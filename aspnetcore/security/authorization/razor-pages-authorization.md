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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core Razor Pages yetkilendirme kuralları

::: moniker range=">= aspnetcore-3.0"

Razor Pages uygulamanızda erişimi denetlemeye yönelik bir yol, başlangıçta yetkilendirme kurallarını kullanmaktır. Bu kurallar, kullanıcıları yetkilendirmeniz ve anonim kullanıcıların ayrı sayfalara veya sayfa klasörlerine erişmesine izin verir. Bu konu başlığı altında açıklanan kurallar, erişimi denetlemek için otomatik olarak [Yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) uygular.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını](xref:security/authentication/cookie)kullanır. Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir. ASP.NET Core kimliği kullanmak için <xref:security/authentication/identity>yönergeleri izleyin.

## <a name="require-authorization-to-access-a-page"></a>Bir sayfaya erişmek için yetkilendirme gerektir

Belirtilen yoldaki sayfaya bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek Için bir [authorizepage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)kullanın:

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, `[Authorize]` Filter özniteliğiyle bir sayfa modeli sınıfına uygulanabilir. Daha fazla bilgi için bkz. [Yetkilendirme filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Bir sayfa klasörüne erişmek için yetkilendirme gerektir

Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [authorizefolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)kullanın:

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Alan sayfasına erişmek için yetkilendirme gerektir

Belirtilen yoldaki alan sayfasına bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> kuralını kullanın:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Sayfa adı, belirtilen alanın sayfalar kök dizinine göre uzantısı olmayan dosyanın yoludur. Örneğin, dosya *alanı/Identity/Pages/Manage/accounts. cshtml* için sayfa adı */Manage/accounts*olur.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareapage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)kullanın:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Bir alan klasörüne erişmek için yetkilendirme gerektir

Belirtilen yoldaki bir klasördeki tüm alanlara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> kuralını kullanın:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Klasör yolu, belirtilen alanın sayfalar kök dizinine göre klasörün yoludur. Örneğin, *bölgeler/kimlik/sayfalar/Yönet/* \ *Yönet*altındaki dosyalar için klasör yolu.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareafolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)kullanın:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Bir sayfaya anonim erişime izin ver

Belirtilen yoldaki bir sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Bir sayfa klasörüne anonim erişime izin ver

Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Yetkili ve anonim erişimi birleştirme hakkında

Bir sayfa klasörünün yetkilendirme gerektirdiğini ve sonra bu klasörün içindeki bir sayfanın adsız erişime izin verdiğini belirtmek için geçerlidir:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Ancak, tersi de geçerlidir. Anonim erişim için bir sayfa klasörü bildiremezsiniz ve ardından bu klasör içinde yetkilendirme gerektiren bir sayfa belirtemezsiniz:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Özel sayfada yetkilendirme gerektirme başarısız olur. Sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ve <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> her ikisi de uygulandığında <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> önceliklidir ve erişimi denetler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor Pages uygulamanızda erişimi denetlemeye yönelik bir yol, başlangıçta yetkilendirme kurallarını kullanmaktır. Bu kurallar, kullanıcıları yetkilendirmeniz ve anonim kullanıcıların ayrı sayfalara veya sayfa klasörlerine erişmesine izin verir. Bu konu başlığı altında açıklanan kurallar, erişimi denetlemek için otomatik olarak [Yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) uygular.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını](xref:security/authentication/cookie)kullanır. Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir. ASP.NET Core kimliği kullanmak için <xref:security/authentication/identity>yönergeleri izleyin.

## <a name="require-authorization-to-access-a-page"></a>Bir sayfaya erişmek için yetkilendirme gerektir

Belirtilen yoldaki sayfaya bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek Için bir [authorizepage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)kullanın:

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, `[Authorize]` Filter özniteliğiyle bir sayfa modeli sınıfına uygulanabilir. Daha fazla bilgi için bkz. [Yetkilendirme filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Bir sayfa klasörüne erişmek için yetkilendirme gerektir

Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [authorizefolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)kullanın:

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Alan sayfasına erişmek için yetkilendirme gerektir

Belirtilen yoldaki alan sayfasına bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> kuralını kullanın:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Sayfa adı, belirtilen alanın sayfalar kök dizinine göre uzantısı olmayan dosyanın yoludur. Örneğin, dosya *alanı/Identity/Pages/Manage/accounts. cshtml* için sayfa adı */Manage/accounts*olur.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareapage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)kullanın:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Bir alan klasörüne erişmek için yetkilendirme gerektir

Belirtilen yoldaki bir klasördeki tüm alanlara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> kuralını kullanın:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Klasör yolu, belirtilen alanın sayfalar kök dizinine göre klasörün yoludur. Örneğin, *bölgeler/kimlik/sayfalar/Yönet/* \ *Yönet*altındaki dosyalar için klasör yolu.

Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareafolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)kullanın:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Bir sayfaya anonim erişime izin ver

Belirtilen yoldaki bir sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Bir sayfa klasörüne anonim erişime izin ver

Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> kuralını kullanın:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Yetkili ve anonim erişimi birleştirme hakkında

Yetkilendirme gerektiren bir sayfa klasörünün ve bu klasörün içindeki bir sayfanın adsız erişime izin verdiğini belirtmek için geçerlidir:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Ancak, tersi de geçerlidir. Anonim erişim için bir sayfa klasörü bildiremezsiniz ve ardından bu klasör içinde yetkilendirme gerektiren bir sayfa belirtemezsiniz:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Özel sayfada yetkilendirme gerektirme başarısız olur. Sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ve <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> her ikisi de uygulandığında <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> önceliklidir ve erişimi denetler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
