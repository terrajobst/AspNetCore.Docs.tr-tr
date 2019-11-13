---
title: ASP.NET Core Razor SDK 'Sı
author: Rick-Anderson
description: ASP.NET Core ' deki Razor Pages, MVC 'yi kullanmaktan daha kolay ve daha üretken hale getirmeye nasıl yardımcı olduğunu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 2fbdf95d02d7918236981c7fee8ebcbedf5c55e1
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963257"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK 'Sı

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

## <a name="overview"></a>Genel bakış

[!INCLUDE[](~/includes/2.1-SDK.md)], `Microsoft.NET.Sdk.Razor` MSBuild SDK 'sını (Razor SDK) içerir. Razor SDK 'Sı:

::: moniker range=">= aspnetcore-3.0"

* ASP.NET Core MVC tabanlı veya [Blazor](xref:blazor/index) projeleri için [Razor](xref:mvc/views/razor) dosyaları içeren projeleri derlemek, paketlemek ve yayımlamak için gereklidir.
* , Razor ( *. cshtml* veya *. Razor*) dosyalarının derlemesini özelleştirmeye izin veren bir dizi önceden tanımlanmış hedef, özellik ve öğe içerir.

Razor SDK, `**\*.cshtml` ve `**\*.razor` glob desenlerine ayarlanmış `Include` öznitelikleri olan `Content` öğelerini içerir. Eşleşen dosyalar yayımlandı.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* ASP.NET Core MVC tabanlı projeler için [Razor](xref:mvc/views/razor) dosyaları içeren projeleri oluşturma, paketleme ve yayımlama ile ilgili deneyimi standartlaştırır.
* Razor dosyalarının derlemesini özelleştirmeye izin veren bir dizi önceden tanımlanmış hedef, özellik ve öğe içerir.

Razor SDK, `**\*.cshtml` glob düzenine ayarlanmış bir `Include` özniteliğine sahip `Content` öğesi içerir. Eşleşen dosyalar yayımlandı.

::: moniker-end

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Razor SDK 'sını kullanma

Çoğu Web uygulaması Razor SDK 'ya açıkça başvurmak için gerekli değildir.

::: moniker range=">= aspnetcore-3.0"

Razor görünümlerini veya Razor Pages içeren sınıf kitaplıkları oluşturmak için Razor SDK 'yı kullanmak için Razor sınıf kitaplığı (RCL) proje şablonuyla başlamasını öneririz. Blazor ( *. Razor*) dosyalarını derlemek için kullanılan bir RCL, [Microsoft. Aspnetcore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) paketine en az bir başvuru gerektirir. Razor görünümlerini veya sayfalarını ( *. cshtml* dosyaları) derlemek için kullanılan bir rcl, `netcoreapp3.0` veya üzeri hedeflemeyi gerektirir ve proje dosyasındaki [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app) öğesine `FrameworkReference` ' ye sahip olmalıdır.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor görünümlerini veya Razor Pages içeren sınıf kitaplıkları derlemek için Razor SDK 'yı kullanmak için:

* `Microsoft.NET.Sdk`yerine `Microsoft.NET.Sdk.Razor` kullanın:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Genellikle, Razor Pages ve Razor görünümlerini derlemek ve derlemek için gerekli ek bağımlılıklar almak için `Microsoft.AspNetCore.Mvc` ' a yönelik bir paket başvurusu gerekir. En azından, projenizin paket başvurularını şu şekilde eklemesi gerekir:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  `Microsoft.AspNetCore.Razor.Design` paketi, proje için Razor derleme görevlerini ve hedeflerini sağlar.

  Önceki paketler `Microsoft.AspNetCore.Mvc` ' a dahildir. Aşağıdaki biçimlendirme, bir ASP.NET Core Razor Pages uygulaması için Razor dosyaları derlemek için Razor SDK 'sını kullanan bir proje dosyası gösterir:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` ve `Microsoft.AspNetCore.Mvc.Razor.Extensions` paketleri [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)içinde yer alır. Ancak, sürüm-daha az `Microsoft.AspNetCore.App` paket başvurusu, en son `Microsoft.AspNetCore.Razor.Design` sürümünü içermeyen uygulamaya bir metapackage sağlar. , Razor için en son derleme zamanı düzeltmelerinin dahil olması için projeler `Microsoft.AspNetCore.Razor.Design` ' ın (veya `Microsoft.AspNetCore.Mvc`) tutarlı bir sürümüne başvurmalıdır. Daha fazla bilgi için [Bu GitHub sorununa](https://github.com/aspnet/Razor/issues/2553)bakın.

::: moniker-end

### <a name="properties"></a>Özellikler

Aşağıdaki özellikler, bir proje derlemesinin parçası olarak Razor SDK davranışını denetler:

* `RazorCompileOnBuild` &ndash; `true` olduğunda, proje oluşturmanın bir parçası olarak Razor derlemesini derler ve yayar. Varsayılan değer `true` ' dır.
* `RazorCompileOnPublish` &ndash; `true` olduğunda, projenin yayımlaması kapsamında Razor derlemesini derler ve yayar. Varsayılan değer `true` ' dır.

Aşağıdaki tablodaki Özellikler ve öğeler, Razor SDK 'ya giriş ve çıkış yapılandırmak için kullanılır.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ASP.NET Core 3,0 ' den başlayarak, MVC görünümleri veya Razor Pages, proje dosyasındaki `RazorCompileOnBuild` veya `RazorCompileOnPublish` MSBuild özellikleri devre dışı bırakılmışsa varsayılan olarak sunulmuyor. Uygulama, *. cshtml* dosyalarını işlemek üzere çalışma zamanı derlemesini kullanıyorsa, uygulamalar [Microsoft. Aspnetcore. Mvc. Razor. runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) paketine açık bir başvuru eklememelidir.

::: moniker-end

| Öğeler | Açıklama |
| ----- | ----------- |
| `RazorGenerate` | Kod oluşturmaya giriş olan öğe öğeleri ( *. cshtml* dosyaları). |
| `RazorComponent` | Razor bileşen kodu oluşturmaya giriş olan öğe öğeleri ( *. Razor* dosyaları). |
| `RazorCompile` | Razor derleme hedeflerine giriş olan öğe öğeleri ( *. cs* dosyaları). Razor derlemesine derlenecek ek dosyaları belirtmek için bu `ItemGroup` kullanın. |
| `RazorTargetAssemblyAttribute` | Razor derlemesi için kod oluşturma öznitelikleri için kullanılan öğe öğeleri. Örneğin:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Oluşturulan Razor derlemesine gömülü kaynaklar olarak eklenen öğe öğeleri. |

| Özellik | Açıklama |
| -------- | ----------- |
| `RazorTargetName` | Razor tarafından üretilen derlemenin dosya adı (uzantısı olmadan). | 
| `RazorOutputPath` | Razor çıkış dizini. |
| `RazorCompileToolset` | Razor derlemesini derlemek için kullanılan araç takımını tespit etmek için kullanılır. Geçerli değerler `Implicit`, `RazorSDK` ve `PrecompilationTool` ' dir. |
| [Enabledefaultcontentıtems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Varsayılan değer `true`. `true`, *Web. config*, *. JSON*ve *. cshtml* dosyalarını projeye içerik olarak ekler. `Microsoft.NET.Sdk.Web`aracılığıyla başvuruluyorsa, *Wwwroot* ve yapılandırma dosyaları altındaki dosyalar da dahil edilir. |
| `EnableDefaultRazorGenerateItems` | `true`, `RazorGenerate` öğelerinde `Content` öğelerden *. cshtml* dosyalarını ekler. |
| `GenerateRazorTargetAssemblyInfo` | `true`, `RazorAssemblyAttribute` tarafından belirtilen öznitelikleri içeren bir *. cs* dosyası oluşturur ve derleme çıkışında dosyayı içerir. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | `true`, `RazorAssemblyAttribute`derleme özniteliklerinin varsayılan bir kümesini ekler. |
| `CopyRazorGenerateFilesToPublishDirectory` | `true`, `RazorGenerate` öğeleri ( *. cshtml*) dosyalarını Yayımla dizinine kopyalar. Genellikle, derleme zamanında veya yayımlama zamanında derlemeye katılırsanız yayımlanmış bir uygulama için Razor dosyaları gerekli değildir. Varsayılan değer `false` ' dır. |
| `CopyRefAssembliesToPublishDirectory` | `true`, başvuru derleme öğelerini yayımlama dizinine kopyalayın. Genellikle, derleme zamanında veya yayımlama zamanında Razor derlemesi gerçekleşirse, yayımlanan bir uygulama için başvuru derlemeleri gerekli değildir. Yayımlanmış uygulamanız çalışma zamanı derlemesi gerektiriyorsa, `true` olarak ayarlayın. Örneğin, uygulama çalışma zamanında *. cshtml* dosyalarını değiştirirse veya gömülü görünümleri kullanıyorsa değeri `true` olarak ayarlayın. Varsayılan değer `false` ' dır. |
| `IncludeRazorContentInPack` | `true`, tüm Razor içerik öğeleri ( *. cshtml* dosyaları) oluşturulan NuGet paketine eklenmek üzere işaretlenir. Varsayılan değer `false` ' dır. |
| `EmbedRazorGenerateSources` | `true`, oluşturulan Razor derlemesine gömülü dosyalar olarak RazorGenerate ( *. cshtml*) öğelerini ekler. Varsayılan değer `false` ' dır. |
| `UseRazorBuildServer` | `true`, kod oluşturma işinin yükünü boşaltmak için kalıcı bir yapı sunucusu işlemi kullanır. Varsayılan değer `UseSharedCompilation` ' dır. |
| `GenerateMvcApplicationPartsAssemblyAttributes` | `true`, SDK, uygulama bölümü keşfi gerçekleştirmek için çalışma zamanında MVC tarafından kullanılan ek öznitelikler üretir. |

Özellikler hakkında daha fazla bilgi için bkz. [MSBuild özellikleri](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Hedefler

Razor SDK iki birincil hedefi tanımlar:

* `RazorGenerate` &ndash; kod `RazorGenerate` öğe öğelerinden *. cs* dosyaları oluşturur. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorGenerateDependsOn` özelliğini kullanın.
* `RazorCompile` &ndash; bir Razor derlemesinde oluşturulan *. cs* dosyalarını derler. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorCompileDependsOn` kullanın.
* `RazorComponentGenerate` &ndash; kod `RazorComponent` öğe öğeleri için *. cs* dosyaları oluşturur. Bu hedeften önce veya sonra çalışabilecek ek hedefleri belirtmek için `RazorComponentGenerateDependsOn` özelliğini kullanın.

### <a name="runtime-compilation-of-razor-views"></a>Razor görünümlerinin çalışma zamanı derlemesi

* Varsayılan olarak, Razor SDK, çalışma zamanı derlemesini gerçekleştirmek için gerekli olan başvuru derlemelerini yayımlamaz. Bu durum, uygulama modeli bir çalışma zamanı derlemesini kullandığında derleme hatalarıyla sonuçlanır. Örneğin, uygulama yayımlandıktan sonra katıştırılmış görünümleri veya değişiklik görünümlerini kullanır. Başvuru derlemelerini yayımlamaya devam etmek için `CopyRefAssembliesToPublishDirectory` ' i `true` olarak ayarlayın.

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
