---
title: Razor dosya derlemeyi ve kod içinde ASP.NET Core ön derlemesi
author: rick-anderson
description: Razor dosyaları ve Razor dosyası ön derleme içinde ASP.NET Core uygulaması yerine getirmeyi önceden derleme avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 2720708f8e58fdc55b82bfb56665005170e79934
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889762"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core Razor dosyası derleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

İlişkili bir MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir. Derleme zamanı Razor dosya yayımlama desteklenmiyor. Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir. Derleme zamanı Razor dosya yayımlama desteklenmiyor. Razor dosyaları isteğe bağlı olarak derlenmesi sırasında zaman yayımlama ve uygulama ile birlikte dağıtılan&mdash;ön derleme aracını kullanarak.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

İlişkili bir Razor sayfası veya MVC görünümü çağrıldığında bir Razor dosya çalışma zamanında derlenir. Razor dosyaları her iki yapı derlenir ve saat kullanarak yayımlama [Razor SDK](xref:razor-pages/sdk).

::: moniker-end

## <a name="precompilation-considerations"></a>Ön derleme konuları

Razor dosyaları önceden derleme yan etkileri verilmiştir:

* Bir küçük yayımlanan paket
* Hızlı bir başlatma süresi
* Razor dosyaları düzenleyemezsiniz&mdash;ilişkili içeriği eksik yayımlanan paket öğesinden.

## <a name="deploy-precompiled-files"></a>Önceden derlenmiş dosyaları dağıtma

::: moniker range=">= aspnetcore-2.1"

Razor dosyaları derleme ve yayımlama-zamanı derlemesini Razor SDK'sı tarafından varsayılan olarak etkindir. Bunlar güncelleştirdikten sonra Razor dosyaları düzenleme, derleme sırasında desteklenir. Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir *.cshtml* dosyaları, uygulamanızla birlikte dağıtılır.

> [!IMPORTANT]
> ASP.NET Core 3. 0'ön derleme aracı kaldırılır. Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).
>
> Yalnızca proje dosyasında hiçbir ön derleme özgü özellikler ayarlandığında Razor SDK'sı etkili olur. Örneğin, ayarı *.csproj* dosyanın `MvcRazorCompileOnPublish` özelliğini `true` Razor SDK'yı devre dışı bırakır.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

.NET Core projenizi olmasını istiyorsanız, hiçbir değişiklik gereklidir.

ASP.NET Core 2.x proje şablonları örtük olarak ayarlanır `MvcRazorCompileOnPublish` özelliğini `true` varsayılan olarak. Sonuç olarak, bu öğe güvenli bir şekilde alanından kaldırılabilir *.csproj* dosya.

> [!IMPORTANT]
> ASP.NET Core 3. 0'ön derleme aracı kaldırılır. Geçiş öneririz [Razor Sdk](xref:razor-pages/sdk).
>
> Razor dosyası ön derleme, gerçekleştirirken kullanılamıyorsa bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Ayarlama `MvcRazorCompileOnPublish` özelliğini `true`, yükleyip [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketi. Aşağıdaki *.csproj* örnek bu ayarları vurgular:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI komutu yayımlama](/dotnet/core/tools/dotnet-publish). Örneğin, proje kök dizinini aşağıdaki komutu yürütün:

```console
dotnet publish -c Release
```

A *< project_name >. PrecompiledViews.dll* derlenmiş Razor dosyalarını içeren dosya, bir ön derleme başarılı olduğunda oluşturulur. Örneğin, aşağıdaki ekran içeriğini gösterir *Index.cshtml* içinde *WebApplication1.PrecompiledViews.dll*:

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a>Razor dosyalarda değişiklik yeniden derleyin

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` Razor dosyaları (Razor görünümleri ve Razor sayfaları) ve yeniden derlenen dosyalar diskte değiştirirseniz güncelleştirilmiş belirleyen bir değer alır veya ayarlar.

Ayarlandığında `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) gözcüler Razor dosyalarında değişiklikler için yapılandırılmış <xref:Microsoft.Extensions.FileProviders.IFileProvider> örnekleri.

Varsayılan değer `true` için:

* ASP.NET Core 2.1 veya daha eski uygulamaları.
* ASP.NET Core 2.2 veya üzeri uygulamaları geliştirme ortamında.

`AllowRecompilingViewsOnFileChange` Uyumluluk anahtarıyla ilişkili olan ve uygulama için yapılandırılan uyumluluk sürümüne bağlı olarak farklı bir davranış sağlayabilir. Ayarlayarak, uygulama yapılandırma `AllowRecompilingViewsOnFileChange` uygulamanın uyumluluk sürümü tarafından kapsanan değeri önceliklidir.

Cihazın uyumluluk sürümü ayarlanırsa <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> veya daha önce `AllowRecompilingViewsOnFileChange` ayarlanır `true` açıkça şekilde yapılandırılmadıkça.

Cihazın uyumluluk sürümü ayarlanırsa `CompatibilityVersion.Version_2_2` ya da sonraki `AllowRecompilingViewsOnFileChange` ayarlanır `false` değeri açıkça yapılandırılmış ya da geliştirme ortamıdır.

Yönergeler ve cihazın uyumluluk sürümü ayarlama örnekleri için bkz. <xref:mvc/compatibility-version>.

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
