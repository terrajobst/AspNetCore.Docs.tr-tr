---
title: Razor dosya derleme ve ASP.NET Core içinde ön derlemesi
author: rick-anderson
description: Razor dosyaları ve ASP.NET Core uygulamasında Razor dosyası ön derlemesi yüklemenin nasıl yapılacağını önceden derleme avantajları hakkında bilgi edinin.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 03b11116a15c291452acd878e32cd015dc553dcc
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34336284"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core Razor dosyası derleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"
İlişkili MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir. Derleme zamanı Razor dosya yayımlama desteklenmiyor. Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile&mdash;ön derleme aracını kullanarak.
::: moniker-end
::: moniker range="= aspnetcore-2.0"
İlişkili Razor sayfasını veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir. Derleme zamanı Razor dosya yayımlama desteklenmiyor. Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile&mdash;ön derleme aracını kullanarak.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
İlişkili Razor sayfasını veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir. Razor dosyaları her iki derleme sırasında derlenir ve zaman kullanarak yayımlamak [Razor SDK](xref:mvc/razor-pages/sdk).
::: moniker-end

## <a name="precompilation-considerations"></a>Ön derleme dikkat edilecek noktalar

Razor dosyaları önceden derleme yan etkileri verilmiştir:

* Bir küçük yayımlanan paket
* Daha hızlı bir başlangıç zamanı
* Razor dosyalarını düzenleyemezsiniz&mdash;ilişkili içeriği eksik yayımlanan paket gelen.

## <a name="deploy-precompiled-files"></a>Önceden derlenmiş dosyaları dağıtma

::: moniker range=">= aspnetcore-2.1"
Razor dosyaları oluşturma ve yayımlama-zamanı derlenmesini Razor SDK'sı tarafından varsayılan olarak etkindir. Bunlar güncelleştirdikten sonra Razor dosyaları düzenleme derleme zamanında desteklenir. Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir *.cshtml* dosyaları ile uygulamanızı dağıtılır.

> [!IMPORTANT]
> Yalnızca ön derlemesi özgü özellikleri proje dosyasında ayarlandığında Razor SDK'sı etkili olur. Örneğin, ayarı *.csproj* dosyanın `MvcRazorCompileOnPublish` özelliğine `true` Razor SDK'yı devre dışı bırakır.
::: moniker-end

::: moniker range="= aspnetcore-2.0"
Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Projeniz .NET Core hedefliyorsa, hiçbir değişiklik gereklidir.

ASP.NET Core 2.x proje şablonları örtük olarak ayarlamak `MvcRazorCompileOnPublish` özelliğine `true` varsayılan olarak. Sonuç olarak, bu öğe güvenle alanından kaldırılabilir *.csproj* dosya.

> [!IMPORTANT]
> Razor dosyası ön derlemesi, gerçekleştirirken kullanılamıyorsa bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.
::: moniker-end

::: moniker range="= aspnetcore-1.1"
Ayarlama `MvcRazorCompileOnPublish` özelliğine `true`, yükleyip [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi. Aşağıdaki *.csproj* örnek bu ayarları vurgular:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI yayımlama komutu](/dotnet/core/tools/dotnet-publish). Örneğin, proje kökündeki aşağıdaki komutu yürütün:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* derlenmiş Razor dosyalarını içeren dosya ön derlemesi başarılı olduğunda oluşturulur. Örneğin, aşağıdaki ekran görüntüsünde içeriğini gösterir *Index.cshtml* içinde *WebApplication1.PrecompiledViews.dll*:

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:mvc/razor-pages/index>
* <xref:mvc/views/overview>
* <xref:mvc/razor-pages/sdk>
::: moniker-end