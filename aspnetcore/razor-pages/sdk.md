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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660071"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK'sı

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Genel Bakış

[!INCLUDE[](~/includes/2.1-SDK.md)], `Microsoft.NET.Sdk.Razor` MSBuild SDK 'sını (Razor SDK) içerir. Razor SDK:

::: moniker range=">= aspnetcore-3.0"

* ASP.NET Core MVC tabanlı veya [Blazor](xref:blazor/index) projeleri için [Razor](xref:mvc/views/razor) dosyaları içeren projeleri derlemek, paketlemek ve yayımlamak için gereklidir.
* , Razor ( *. cshtml* veya *. Razor*) dosyalarının derlemesini özelleştirmeye izin veren bir dizi önceden tanımlanmış hedef, özellik ve öğe içerir.

Razor SDK, `**\*.cshtml` ve `**\*.razor` glob desenlerine ayarlı `Include` öznitelikleri olan `Content` öğeleri içerir. Eşleşen dosyalar yayımlandı.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* ASP.NET Core MVC tabanlı projeler için [Razor](xref:mvc/views/razor) dosyaları içeren projeleri oluşturma, paketleme ve yayımlama ile ilgili deneyimi standartlaştırır.
* Bir dizi önceden tanımlanmış hedefleri, özellikler ve Razor dosyaları derleme özelleştirme izin veren öğeleri içerir.

Razor SDK, `**\*.cshtml` glob düzenine ayarlanmış bir `Include` özniteliğine sahip `Content` öğesi içerir. Eşleşen dosyalar yayımlandı.

::: moniker-end

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Razor SDK 'sını kullanma

Çoğu web uygulaması açıkça Razor SDK'ya başvurmak için gerekli değildir.

::: moniker range=">= aspnetcore-3.0"

Razor görünümlerini veya Razor Pages içeren sınıf kitaplıkları oluşturmak için Razor SDK 'yı kullanmak için Razor sınıf kitaplığı (RCL) proje şablonuyla başlamasını öneririz. Blazor ( *. Razor*) dosyalarını derlemek için kullanılan bir RCL, [Microsoft. Aspnetcore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) paketine en az bir başvuru gerektirir. Razor görünümlerini veya sayfalarını ( *. cshtml* dosyaları) derlemek için kullanılan bir rcl, `netcoreapp3.0` veya üzeri hedefleme gerektirir ve proje dosyasındaki [Microsoft. Aspnetcore. App metapackage](xref:fundamentals/metapackage-app) `FrameworkReference`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor görünümleri ya da Razor sayfaları içeren sınıf kitaplıkları oluşturmak için Razor SDK'sını kullanmak için:

* `Microsoft.NET.Sdk`yerine `Microsoft.NET.Sdk.Razor` kullanın:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Genellikle, Razor Pages ve Razor görünümlerini derlemek ve derlemek için gereken ek bağımlılıklar almak için `Microsoft.AspNetCore.Mvc` bir paket başvurusu gerekir. En azından, paket başvuruları projenize eklemeniz gerekir:

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  `Microsoft.AspNetCore.Razor.Design` paketi, proje için Razor derleme görevlerini ve hedeflerini sağlar.

  Önceki paketler `Microsoft.AspNetCore.Mvc`eklenmiştir. Aşağıdaki biçimlendirmede Razor dosyaları için bir ASP.NET Core Razor sayfalar uygulama oluşturmak için Razor SDK'sını kullanan bir proje dosyasını göstermektedir:

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` ve `Microsoft.AspNetCore.Mvc.Razor.Extensions` paketleri [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde yer alır. Ancak, sürüm-daha az `Microsoft.AspNetCore.App` paket başvurusu, en son `Microsoft.AspNetCore.Razor.Design`sürümünü içermeyen uygulamaya bir metapackage sağlar. , Razor için en son derleme zamanı düzeltmelerinin dahil olması için projeler `Microsoft.AspNetCore.Razor.Design` tutarlı bir sürüme (veya `Microsoft.AspNetCore.Mvc`) başvurmalıdır. Daha fazla bilgi için [Bu GitHub sorununa](https://github.com/aspnet/Razor/issues/2553)bakın.

::: moniker-end

### <a name="properties"></a>Özellikler

Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışı denetler:

* `true``RazorCompileOnBuild` &ndash;, proje oluşturmanın bir parçası olarak Razor derlemesini derler ve yayar. `true` değerini varsayılan olarak alır.
* `true``RazorCompileOnPublish` &ndash;, projenin yayımlaması kapsamında Razor derlemesini derler ve yayar. `true` değerini varsayılan olarak alır.

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
| `RazorTargetAssemblyAttribute` | Kod için kullanılan öğeler için Razor derleme öznitelikleri oluşturur. Örnek:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Oluşturulan Razor derlemesine katıştırılmış kaynakları olarak eklenen öğeler. |

| Özellik | Açıklama |
| -------- | ----------- |
| `RazorTargetName` | Razor tarafından üretilen derleme dosya adı (uzantısı olmadan). |
| `RazorOutputPath` | Razor çıkış dizini. |
| `RazorCompileToolset` | Razor derlemesi oluşturmak için kullanılan araç kümesini belirlemek için kullanılır. Geçerli değerler `Implicit`, `RazorSDK`ve `PrecompilationTool`. |
| [Enabledefaultcontentıtems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | `true` varsayılan değerdir. `true`, *Web. config*, *. JSON*ve *. cshtml* dosyalarını projeye içerik olarak ekler. `Microsoft.NET.Sdk.Web`aracılığıyla başvuruluyorsa, *Wwwroot* ve yapılandırma dosyaları altındaki dosyalar da dahil edilir. |
| `EnableDefaultRazorGenerateItems` | `true`, `RazorGenerate` öğelerinde `Content` öğelerden *. cshtml* dosyalarını ekler. |
| `GenerateRazorTargetAssemblyInfo` | `true`, `RazorAssemblyAttribute` tarafından belirtilen öznitelikleri içeren bir *. cs* dosyası oluşturur ve derleme çıkışında dosyayı içerir. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | `true`, `RazorAssemblyAttribute`derleme özniteliklerinin varsayılan bir kümesini ekler. |
| `CopyRazorGenerateFilesToPublishDirectory` | `true`, `RazorGenerate` öğeleri ( *. cshtml*) dosyalarını Yayımla dizinine kopyalar. Genellikle, derleme zamanı veya yayımlama zamanı derleme katıldıkları, Razor dosyaları bir yayımlanan uygulama için gerekli değildir. `false` değerini varsayılan olarak alır. |
| `CopyRefAssembliesToPublishDirectory` | `true`, başvuru derleme öğelerini yayımlama dizinine kopyalayın. Genellikle, Razor derleme, derleme zamanı veya yayımlama zamanı oluşursa başvuru bütünleştirilmiş kodları yayınlanmış bir uygulama için gerekli değildir. Yayımlanmış uygulamanız çalışma zamanı derlemesi gerektiriyorsa `true` olarak ayarlayın. Örneğin, uygulama çalışma zamanında *. cshtml* dosyalarını değiştirirse veya gömülü görünümleri kullanıyorsa değeri `true` olarak ayarlayın. `false` değerini varsayılan olarak alır. |
| `IncludeRazorContentInPack` | `true`, tüm Razor içerik öğeleri ( *. cshtml* dosyaları) oluşturulan NuGet paketine eklenmek üzere işaretlenir. `false` değerini varsayılan olarak alır. |
| `EmbedRazorGenerateSources` | `true`, oluşturulan Razor derlemesine gömülü dosyalar olarak RazorGenerate ( *. cshtml*) öğelerini ekler. `false` değerini varsayılan olarak alır. |
| `UseRazorBuildServer` | `true`, kod oluşturma işinin yükünü boşaltmak için kalıcı bir yapı sunucusu işlemi kullanır. Varsayılan olarak `UseSharedCompilation`değerini alır. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | `true`, SDK, uygulama bölümü keşfi gerçekleştirmek için çalışma zamanında MVC tarafından kullanılan ek öznitelikler üretir. |
| `DefaultWebContentItemExcludes` | Web veya Razor SDK 'Yı hedefleyen projelerde `Content` öğesi grubundan çıkarılacak öğe öğeleri için glob bir model |
| `ExcludeConfigFilesFromBuildOutput` | `true`, *. config* ve *. JSON* dosyaları yapı çıkış dizinine kopyalanmaz. |
| `AddRazorSupportForMvc` | `true`,, MVC görünümlerini veya Razor Pages içeren uygulamalar oluştururken gerekli olan MVC yapılandırmasına yönelik destek eklemek için Razor SDK 'sını yapılandırır. Bu özellik, Web SDK 'sını hedefleyen .NET Core 3,0 veya üzeri projeler için örtük olarak ayarlanmıştır |
| `RazorLangVersion` | Hedeflenecek Razor dilinin sürümü. |

Özellikler hakkında daha fazla bilgi için bkz. [MSBuild özellikleri](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Hedefler

Razor SDK'sı iki birincil hedefleri tanımlar:

* `RazorGenerate` &ndash; kodu, `RazorGenerate` öğesi öğelerinden *. cs* dosyaları oluşturur. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorGenerateDependsOn` özelliğini kullanın.
* `RazorCompile` &ndash; oluşturulan *. cs* dosyalarını Razor derlemesinde derler. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorCompileDependsOn` kullanın.
* `RazorComponentGenerate` &ndash; kod `RazorComponent` öğesi öğeleri için *. cs* dosyaları oluşturur. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorComponentGenerateDependsOn` özelliğini kullanın.

### <a name="runtime-compilation-of-razor-views"></a>Razor görünüm çalışma zamanı derlemesi

* Varsayılan olarak, Razor SDK'sı, çalışma zamanı derleme gerçekleştirmek için gereken başvuru derlemelerini yayımlama değil. Bu, uygulama modeli çalışma zamanı derlemesini kullandığında derleme hatalarıyla sonuçlanır&mdash;Örneğin, uygulama yayımlandıktan sonra uygulamanın katıştırılmış görünümleri veya değişiklik görünümlerini kullanır. Başvuru derlemelerini yayımlamaya devam etmek için `CopyRefAssembliesToPublishDirectory` `true` olarak ayarlayın.

* Bir Web uygulaması için uygulamanızın `Microsoft.NET.Sdk.Web` SDK 'Yı hedeflediğinden emin olun.

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
