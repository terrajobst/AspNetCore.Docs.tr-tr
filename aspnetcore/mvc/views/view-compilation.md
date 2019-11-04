---
title: ASP.NET Core 'de Razor dosyası derlemesi
author: rick-anderson
description: ASP.NET Core uygulamasında Razor dosyalarının derlemesini nasıl oluştuğunu öğrenin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/31/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 95fa0d72ed9c088945707ac6b79c3fbde35a5a30
ms.sourcegitcommit: eb2fe5ad2e82fab86ca952463af8d017ba659b25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2019
ms.locfileid: "73416142"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core 'de Razor dosyası derlemesi

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

::: moniker range="= aspnetcore-1.1"

Bir Razor dosyası, ilişkili MVC görünümü çağrıldığında çalışma zamanında derlenir. Derleme zamanı Razor dosyası yayımlama desteklenmiyor. Razor dosyaları isteğe bağlı olarak yayımlama zamanında derlenebilir ve ön derleme Aracı kullanılarak uygulama&mdash;dağıtılabilir.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Razor dosyası, ilişkili Razor sayfası veya MVC görünümü çağrıldığında çalışma zamanında derlenir. Derleme zamanı Razor dosyası yayımlama desteklenmiyor. Razor dosyaları isteğe bağlı olarak yayımlama zamanında derlenebilir ve ön derleme Aracı kullanılarak uygulama&mdash;dağıtılabilir.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Razor dosyası, ilişkili Razor sayfası veya MVC görünümü çağrıldığında çalışma zamanında derlenir. Razor dosyaları, [Razor SDK](xref:razor-pages/sdk)kullanılarak hem derlemede hem de yayımlama zamanında derlenir.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

*. Cshtml* uzantılı Razor dosyaları, [Razor SDK](xref:razor-pages/sdk)kullanılarak hem derlemede hem de yayımlama zamanında derlenir. Çalışma zamanı derlemesi, uygulamanız yapılandırılarak isteğe bağlı olarak etkinleştirilebilir.

::: moniker-end

## <a name="razor-compilation"></a>Razor derleme

::: moniker range=">= aspnetcore-3.0"

Razor dosyalarının derleme ve yayımlama zamanı derlemesi, Razor SDK tarafından varsayılan olarak etkinleştirilmiştir. Etkinleştirildiğinde, çalışma zamanı derlemesi derleme zamanı derlemesini tamamlar ve bunlar düzenlendiklerinde, Razor dosyalarının güncelleştirilmesine izin verir.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Razor dosyalarının derleme ve yayımlama zamanı derlemesi, Razor SDK tarafından varsayılan olarak etkinleştirilmiştir. Razor dosyalarını güncelleştirildikten sonra düzenlediğinizde derleme zamanında desteklenir. Varsayılan olarak, yalnızca derlenen *views. dll* ve No *. cshtml* dosyaları ya da Razor dosyalarını derlemek için gerekli derlemeler, uygulamanız ile dağıtılır.

> [!IMPORTANT]
> Ön derleme aracı kullanım dışı bırakılmıştır ve ASP.NET Core 3,0 ' de kaldırılacak. [Razor SDK](xref:razor-pages/sdk)'ya geçiş yapmanızı öneririz.
>
> Razor SDK yalnızca proje dosyasında ön derleme özgü hiçbir özellik ayarlanmamışsa etkilidir. Örneğin, *. csproj* dosyasının `MvcRazorCompileOnPublish` özelliğinin `true` olarak ayarlanması Razor SDK 'sını devre dışı bırakır.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Projeniz .NET Framework hedefliyorsa [Microsoft. AspNetCore. Mvc. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketini yükler:

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

Projeniz .NET Core 'u hedefliyorsa, hiçbir değişiklik yapılması gerekmez.

ASP.NET Core 2. x proje şablonları, varsayılan olarak `MvcRazorCompileOnPublish` özelliğini `true` olarak ayarlar. Sonuç olarak, bu öğe *. csproj* dosyasından güvenle kaldırılabilir.

> [!IMPORTANT]
> Ön derleme aracı kullanım dışı bırakılmıştır ve ASP.NET Core 3,0 ' de kaldırılacak. [Razor SDK](xref:razor-pages/sdk)'ya geçiş yapmanızı öneririz.
>
> ASP.NET Core 2,0 ' de [otomatik olarak barındırılan bir dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) gerçekleştirilirken Razor dosyası ön derlemesi kullanılamaz.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

`MvcRazorCompileOnPublish` özelliğini `true`olarak ayarlayın ve [Microsoft. AspNetCore. Mvc. Razor. ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet paketini yükler. Aşağıdaki *. csproj* örneği bu ayarları vurgular:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[.NET Core CLI Publish komutuyla](/dotnet/core/tools/dotnet-publish)uygulamayı [çerçeveye bağlı bir dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) için hazırlayın. Örneğin, proje kökünde aşağıdaki komutu yürütün:

```dotnetcli
dotnet publish -c Release
```

Bir *\<project_name >. Derlenen Razor dosyalarını içeren PrecompiledViews. dll* dosyası, ön derleme başarılı olduğunda üretilir. Örneğin, aşağıdaki ekran görüntüsünde *WebApplication1. PrecompiledViews. dll*içindeki *Index. cshtml* 'nin içerikleri gösterilmektedir:

![DLL içinde Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>Çalışma zamanı derlemesi

::: moniker range="= aspnetcore-2.1"

Yapı zamanı derlemesi, Razor dosyalarının çalışma zamanı derlemesi tarafından belirlenir. ASP.NET Core MVC, bir *. cshtml* dosyasının Içeriği değiştiğinde Razor dosyalarını yeniden derleyerek.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Yapı zamanı derlemesi, Razor dosyalarının çalışma zamanı derlemesi tarafından belirlenir. <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>, Razor dosyalarının (Razor görünümleri ve Razor Pages) yeniden derlendiğini ve dosyaların diskte değiştirilmesi durumunda güncelleştirilip güncelleştirilmediğini belirleyen bir değer alır veya ayarlar.

Varsayılan değer için `true`:

* Uygulamanın uyumluluk sürümü <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> veya daha önceki bir sürüme ayarlandıysa
* Uygulamanın uyumluluk sürümü <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> veya üzeri olarak ayarlandıysa ve uygulama geliştirme ortamındaysanız <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>. Diğer bir deyişle, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> açıkça ayarlanmamışsa, Razor dosyaları geliştirme dışı ortamda yeniden derlenmez.

Uygulamanın uyumluluk sürümünü ayarlamaya yönelik rehberlik ve örnekler için bkz. <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Çalışma zamanı derlemesi `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` paketi kullanılarak etkinleştirildi. Çalışma zamanı derlemesini etkinleştirmek için uygulamalar şu şekilde olmalıdır:

* [Microsoft. AspNetCore. Mvc. Razor. RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet paketini yükler.
* Projenin `Startup.ConfigureServices` yöntemini `AddRazorRuntimeCompilation`çağrısı içerecek şekilde güncelleştirin:

  ```csharp
  services
      .AddControllersWithViews()
      .AddRazorRuntimeCompilation();
  ```

::: moniker-end

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
