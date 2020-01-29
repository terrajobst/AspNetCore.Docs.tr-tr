---
title: ASP.NET Core Razor SDK'sı
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 872d90662494735dc0e4caa01c46fcdcc2606bc6
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809139"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK'sı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Genel bakış

[!INCLUDE[](~/includes/2.1-SDK.md)] İçerir `Microsoft.NET.Sdk.Razor` MSBuild SDK'sı (Razor SDK). Razor SDK:

::: moniker range=">= aspnetcore-3.0"

* ASP.NET Core MVC tabanlı veya [Blazor](xref:blazor/index) projeleri için [Razor](xref:mvc/views/razor) dosyaları içeren projeleri derlemek, paketlemek ve yayımlamak için gereklidir.
* , Razor ( *. cshtml* veya *. Razor*) dosyalarının derlemesini özelleştirmeye izin veren bir dizi önceden tanımlanmış hedef, özellik ve öğe içerir.

Razor SDK, `**\*.cshtml` ve `**\*.razor` glob desenlerine ayarlı `Include` öznitelikleri olan `Content` öğeleri içerir. Eşleşen dosyalar yayımlandı.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Oluşturma, paketleme ve içeren proje yayımlama deneyimi standartlaştırır [Razor](xref:mvc/views/razor) dosyaları ASP.NET Core MVC tabanlı projeler için.
* Bir dizi önceden tanımlanmış hedefleri, özellikler ve Razor dosyaları derleme özelleştirme izin veren öğeleri içerir.

Razor SDK, `**\*.cshtml` glob düzenine ayarlanmış bir `Include` özniteliğine sahip `Content` öğesi içerir. Eşleşen dosyalar yayımlandı.

::: moniker-end

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Razor SDK 'sını kullanma

Çoğu web uygulaması açıkça Razor SDK'ya başvurmak için gerekli değildir.

::: moniker range=">= aspnetcore-3.0"

Razor görünümlerini veya Razor Pages içeren sınıf kitaplıkları oluşturmak için Razor SDK 'yı kullanmak için Razor sınıf kitaplığı (RCL) proje şablonuyla başlamasını öneririz. Blazor ( *. Razor*) dosyalarını derlemek için kullanılan bir RCL, [Microsoft. Aspnetcore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) paketine en az bir başvuru gerektirir. Razor görünümlerini veya sayfalarını ( *. cshtml* dosyaları) derlemek için kullanılan bir rcl, `netcoreapp3.0` veya üzeri hedefleme gerektirir ve proje dosyasındaki [Microsoft. Aspnetcore. App metapackage](xref:fundamentals/metapackage-app) `FrameworkReference`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor görünümleri ya da Razor sayfaları içeren sınıf kitaplıkları oluşturmak için Razor SDK'sını kullanmak için:

* Kullanım `Microsoft.NET.Sdk.Razor` yerine `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Genellikle, bir paket başvurusu `Microsoft.AspNetCore.Mvc` oluşturmak ve Razor sayfaları ve Razor görünümleri derlemek için gereken ek bağımlılıklar almak için gereklidir. En azından, paket başvuruları projenize eklemeniz gerekir:

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  `Microsoft.AspNetCore.Razor.Design` Paket proje için Razor derleme görevleri ve hedefleri sağlar.

  Yukarıdaki paketleri dahil `Microsoft.AspNetCore.Mvc`. Aşağıdaki biçimlendirmede Razor dosyaları için bir ASP.NET Core Razor sayfalar uygulama oluşturmak için Razor SDK'sını kullanan bir proje dosyasını göstermektedir:

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` Ve `Microsoft.AspNetCore.Mvc.Razor.Extensions` paketleri dahil edilecek [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Ancak, daha az sürüm `Microsoft.AspNetCore.App` paket başvuru sağlayan bir metapackage en son sürümünü içermeyen App `Microsoft.AspNetCore.Razor.Design`. Projeleri sürümünün tutarlı olmasını başvurmalıdır `Microsoft.AspNetCore.Razor.Design` (veya `Microsoft.AspNetCore.Mvc`) böylece Razor yönelik en son derleme zamanı düzeltmeleri dahil edilir. Daha fazla bilgi için [bu GitHub sorunu](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Özellikler

Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışı denetler:

* `RazorCompileOnBuild` &ndash; Zaman `true`, derler ve Razor derleme proje oluşturma işleminin parçası olarak yayar. `true` değerini varsayılan olarak alır.
* `RazorCompileOnPublish` &ndash; Zaman `true`, derler ve Razor derleme proje yayımlama işleminin parçası olarak yayar. `true` değerini varsayılan olarak alır.

Özellikler ve öğeler aşağıdaki tabloda, giriş ve çıkış Razor SDK'sına yapılandırmak için kullanılır.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ASP.NET Core 3,0 ' den başlayarak, MVC görünümleri veya Razor Pages proje dosyasındaki `RazorCompileOnBuild` veya `RazorCompileOnPublish` MSBuild özellikleri devre dışı bırakılmışsa varsayılan olarak sunulmuyor. Uygulama, *. cshtml* dosyalarını işlemek üzere çalışma zamanı derlemesini kullanıyorsa, uygulamalar [Microsoft. Aspnetcore. Mvc. Razor. runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) paketine açık bir başvuru eklememelidir.

::: moniker-end

| Öğeler | Açıklama |
| ----- | ----------- |
| `RazorGenerate` | Kod oluşturmaya giriş olan öğe öğeleri ( *. cshtml* dosyaları). |
| `RazorComponent` | Razor bileşen kodu oluşturmaya giriş olan öğe öğeleri ( *. Razor* dosyaları). |
| `RazorCompile` | Razor derleme hedeflerine giriş olan öğe öğeleri ( *. cs* dosyaları). Razor derlemesine derlenecek ek dosyaları belirtmek için bu `ItemGroup` kullanın. |
| `RazorTargetAssemblyAttribute` | Kod için kullanılan öğeler için Razor derleme öznitelikleri oluşturur. Örneğin:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Oluşturulan Razor derlemesine katıştırılmış kaynakları olarak eklenen öğeler. |

| Özellik | Açıklama |
| -------- | ----------- |
| `RazorTargetName` | Razor tarafından üretilen derleme dosya adı (uzantısı olmadan). |
| `RazorOutputPath` | Razor çıkış dizini. |
| `RazorCompileToolset` | Razor derlemesi oluşturmak için kullanılan araç kümesini belirlemek için kullanılır. Geçerli değerler `Implicit`, `RazorSDK`, ve `PrecompilationTool`. |
| [Enabledefaultcontentıtems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Varsayılan değer `true`. `true`, *Web. config*, *. JSON*ve *. cshtml* dosyalarını projeye içerik olarak ekler. Aracılığıyla başvurulduğunda `Microsoft.NET.Sdk.Web`, altında dosyaları *wwwroot* ve yapılandırma dosyaları da dahil edilir. |
| `EnableDefaultRazorGenerateItems` | Zaman `true`, içeren *.cshtml* dosyalarını `Content` öğeler `RazorGenerate` öğeleri. |
| `GenerateRazorTargetAssemblyInfo` | Zaman `true`, oluşturur bir *.cs* tarafından belirtilen öznitelikler içeren dosya `RazorAssemblyAttribute` ve derleme çıkış dosyası içerir. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Zaman `true`, derleme öznitelikleri için varsayılan ayarı ekler `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Zaman `true`, kopya `RazorGenerate` öğeleri ( *.cshtml*) dosyalarını yayımlama dizinine kopyalayın. Genellikle, derleme zamanı veya yayımlama zamanı derleme katıldıkları, Razor dosyaları bir yayımlanan uygulama için gerekli değildir. `false` değerini varsayılan olarak alır. |
| `CopyRefAssembliesToPublishDirectory` | Zaman `true`, başvuru bütünleştirilmiş kod öğeleri Yayımla dizinine kopyalayın. Genellikle, Razor derleme, derleme zamanı veya yayımlama zamanı oluşursa başvuru bütünleştirilmiş kodları yayınlanmış bir uygulama için gerekli değildir. Kümesine `true` yayımlanmış uygulamanızın çalışma zamanı derlemesi gerekiyorsa. Örneğin, değer kümesine `true` uygulama değiştirirse *.cshtml* çalışma zamanında dosya veya katıştırılmış görünümlerini kullanır. `false` değerini varsayılan olarak alır. |
| `IncludeRazorContentInPack` | Zaman `true`, tüm Razor içerik öğeleri ( *.cshtml* dosyaları) üretilen NuGet paketini eklenmek üzere işaretlenir. `false` değerini varsayılan olarak alır. |
| `EmbedRazorGenerateSources` | Zaman `true`, RazorGenerate ekler ( *.cshtml*) öğeleri olarak oluşturulmuş Razor derlemesine katıştırılmış dosyaları. `false` değerini varsayılan olarak alır. |
| `UseRazorBuildServer` | Zaman `true`, kod oluşturma iş yüklerini boşaltmak üzere bir sürekli derleme sunucu işlemi kullanır. Varsayılan olarak, değeri olarak `UseSharedCompilation`. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | `true`, SDK, uygulama bölümü keşfi gerçekleştirmek için çalışma zamanında MVC tarafından kullanılan ek öznitelikler üretir. |
| `DefaultWebContentItemExcludes` | Web veya Razor SDK 'Yı hedefleyen projelerde `Content` öğesi grubundan çıkarılacak öğe öğeleri için glob bir model |
| `ExcludeConfigFilesFromBuildOutput` | `true`, *. config* ve *. JSON* dosyaları yapı çıkış dizinine kopyalanmaz. |
| `AddRazorSupportForMvc` | `true`,, MVC görünümlerini veya Razor Pages içeren uygulamalar oluştururken gerekli olan MVC yapılandırmasına yönelik destek eklemek için Razor SDK 'sını yapılandırır. Bu özellik, Web SDK 'sını hedefleyen .NET Core 3,0 veya üzeri projeler için örtük olarak ayarlanmıştır |
| `RazorLangVersion` | Hedeflenecek Razor dilinin sürümü. |

Özellikler hakkında daha fazla bilgi için bkz. [MSBuild özellikleri](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Hedefler

Razor SDK'sı iki birincil hedefleri tanımlar:

* `RazorGenerate` &ndash; Kod oluşturur *.cs* dosyalarını `RazorGenerate` öğesi öğeleri. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorGenerateDependsOn` özelliğini kullanın.
* `RazorCompile` &ndash; Oluşturulan derler *.cs* için bir Razor derleme dosyaları. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorCompileDependsOn` kullanın.
* `RazorComponentGenerate` &ndash; kod `RazorComponent` öğesi öğeleri için *. cs* dosyaları oluşturur. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorComponentGenerateDependsOn` özelliğini kullanın.

### <a name="runtime-compilation-of-razor-views"></a>Razor görünüm çalışma zamanı derlemesi

* Varsayılan olarak, Razor SDK'sı, çalışma zamanı derleme gerçekleştirmek için gereken başvuru derlemelerini yayımlama değil. Çalışma zamanı derleme sırasında uygulama modeli kullanır, bu derleme hataları sonuçları&mdash;Örneğin, uygulama yayımlandıktan sonra uygulamayı katıştırılmış görünümleri ya da değişiklikleri görünümler kullanır. Ayarlama `CopyRefAssembliesToPublishDirectory` için `true` başvuru bütünleştirilmiş kodları yayımlamaya devam etmek için.

* Bir web uygulaması için uygulamanızın hedeflediği olun `Microsoft.NET.Sdk.Web` SDK.

## <a name="razor-language-version"></a>Razor dili sürümü

`Microsoft.NET.Sdk.Web` SDK 'Sı hedeflenirken, Razor dili sürümü uygulamanın hedef Framework sürümünden algılanır. `Microsoft.NET.Sdk.Razor` SDK 'Yı hedefleyen projeler veya uygulamanın çıkarılan değerden farklı bir Razor dili sürümü gerektirmesi durumunda, uygulamanın proje dosyasındaki `<RazorLangVersion>` özelliği ayarlanarak bir sürüm yapılandırılabilir:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Razor 'nin dil sürümü, için oluşturulduğu çalışma zamanının sürümü ile sıkı bir şekilde tümleşiktir. Çalışma zamanı için tasarlanmamış bir dil sürümünü hedeflemek desteklenmez ve muhtemelen derleme hataları üretir.

## <a name="additional-resources"></a>Ek kaynaklar

* [.NET Core için csproj biçimine eklemeler](/dotnet/core/tools/csproj)
* [Ortak MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items)
