---
title: ASP.NET Core Razor SDK'sı
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: de51c9443e639cd64c234b6975cf7252bb7a2b9a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901895"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK'sı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1-SDK.md)] İçerir `Microsoft.NET.Sdk.Razor` MSBuild SDK'sı (Razor SDK). Razor SDK:

* Oluşturma, paketleme ve içeren proje yayımlama deneyimi standartlaştırır [Razor](xref:mvc/views/razor) dosyaları ASP.NET Core MVC tabanlı projeler için.
* Bir dizi önceden tanımlanmış hedefleri, özellikler ve Razor dosyaları derleme özelleştirme izin veren öğeleri içerir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Razor SDK'sını kullanma

Çoğu web uygulaması açıkça Razor SDK'ya başvurmak için gerekli değildir.

Razor görünümleri ya da Razor sayfaları içeren sınıf kitaplıkları oluşturmak için Razor SDK'sını kullanmak için:

* Kullanım `Microsoft.NET.Sdk.Razor` yerine `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* Genellikle, bir paket başvurusu `Microsoft.AspNetCore.Mvc` oluşturmak ve Razor sayfaları ve Razor görünümleri derlemek için gereken ek bağımlılıklar almak için gereklidir. En azından, paket başvuruları projenize eklemeniz gerekir:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  `Microsoft.AspNetCore.Razor.Design` Paket proje için Razor derleme görevleri ve hedefleri sağlar.

  Yukarıdaki paketleri dahil `Microsoft.AspNetCore.Mvc`. Aşağıdaki biçimlendirmede Razor dosyaları için bir ASP.NET Core Razor sayfalar uygulama oluşturmak için Razor SDK'sını kullanan bir proje dosyasını göstermektedir:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` Ve `Microsoft.AspNetCore.Mvc.Razor.Extensions` paketleri dahil edilecek [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Ancak, daha az sürüm `Microsoft.AspNetCore.App` paket başvuru sağlayan bir metapackage en son sürümünü içermeyen App `Microsoft.AspNetCore.Razor.Design`. Projeleri sürümünün tutarlı olmasını başvurmalıdır `Microsoft.AspNetCore.Razor.Design` (veya `Microsoft.AspNetCore.Mvc`) böylece Razor yönelik en son derleme zamanı düzeltmeleri dahil edilir. Daha fazla bilgi için [bu GitHub sorunu](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Özellikler

Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışı denetler:

* `RazorCompileOnBuild` &ndash; Zaman `true`, derler ve Razor derleme proje oluşturma işleminin parçası olarak yayar. Varsayılan olarak `true`.
* `RazorCompileOnPublish` &ndash; Zaman `true`, derler ve Razor derleme proje yayımlama işleminin parçası olarak yayar. Varsayılan olarak `true`.

Özellikler ve öğeler aşağıdaki tabloda, giriş ve çıkış Razor SDK'sına yapılandırmak için kullanılır.

| Öğeler | Açıklama |
| ----- | ----------- |
| `RazorGenerate` | Öğe öğeleri (*.cshtml* dosyaları) girişleri kod oluşturma hedefleri olan. |
| `RazorCompile` | Öğe öğeleri (*.cs* dosyaları) girişleri Razor derleme hedefleri olan. Bu ItemGroup Razor bütünleştirilmiş kod içine derlenmiş için ek dosyaları belirtmek için kullanın. |
| `RazorTargetAssemblyAttribute` | Kod için kullanılan öğeler için Razor derleme öznitelikleri oluşturur. Örneğin:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Oluşturulan Razor derlemesine katıştırılmış kaynakları olarak eklenen öğeler. |

| Özellik | Açıklama |
| -------- | ----------- |
| `RazorTargetName` | Razor tarafından üretilen derleme dosya adı (uzantısı olmadan). | 
| `RazorOutputPath` | Razor çıkış dizini. |
| `RazorCompileToolset` | Razor derlemesi oluşturmak için kullanılan araç kümesini belirlemek için kullanılır. Geçerli değerler `Implicit`, `RazorSDK`, ve `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Varsayılan değer `true`. Zaman `true`, içeren *web.config*, *.json*, ve *.cshtml* içerik olarak proje dosyaları. Aracılığıyla başvurulduğunda `Microsoft.NET.Sdk.Web`, altında dosyaları *wwwroot* ve yapılandırma dosyaları da dahil edilir. |
| `EnableDefaultRazorGenerateItems` | Zaman `true`, içeren *.cshtml* dosyalarını `Content` öğeler `RazorGenerate` öğeleri. |
| `GenerateRazorTargetAssemblyInfo` | Zaman `true`, oluşturur bir *.cs* tarafından belirtilen öznitelikler içeren dosya `RazorAssemblyAttribute` ve derleme çıkış dosyası içerir. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Zaman `true`, derleme öznitelikleri için varsayılan ayarı ekler `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Zaman `true`, kopya `RazorGenerate` öğeleri (*.cshtml*) dosyalarını yayımlama dizinine kopyalayın. Genellikle, derleme zamanı veya yayımlama zamanı derleme katıldıkları, Razor dosyaları bir yayımlanan uygulama için gerekli değildir. Varsayılan olarak `false`. |
| `CopyRefAssembliesToPublishDirectory` | Zaman `true`, başvuru bütünleştirilmiş kod öğeleri Yayımla dizinine kopyalayın. Genellikle, Razor derleme, derleme zamanı veya yayımlama zamanı oluşursa başvuru bütünleştirilmiş kodları yayınlanmış bir uygulama için gerekli değildir. Kümesine `true` yayımlanmış uygulamanızın çalışma zamanı derlemesi gerekiyorsa. Örneğin, değer kümesine `true` uygulama değiştirirse *.cshtml* çalışma zamanında dosya veya katıştırılmış görünümlerini kullanır. Varsayılan olarak `false`. |
| `IncludeRazorContentInPack` | Zaman `true`, tüm Razor içerik öğeleri (*.cshtml* dosyaları) üretilen NuGet paketini eklenmek üzere işaretlenir. Varsayılan olarak `false`. |
| `EmbedRazorGenerateSources` | Zaman `true`, RazorGenerate ekler (*.cshtml*) öğeleri olarak oluşturulmuş Razor derlemesine katıştırılmış dosyaları. Varsayılan olarak `false`. |
| `UseRazorBuildServer` | Zaman `true`, kod oluşturma iş yüklerini boşaltmak üzere bir sürekli derleme sunucu işlemi kullanır. Varsayılan olarak, değeri olarak `UseSharedCompilation`. |

Özellikler hakkında daha fazla bilgi için bkz. [MSBuild özellikleri](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Hedefler

Razor SDK'sı iki birincil hedefleri tanımlar:

* `RazorGenerate` &ndash; Kod oluşturur *.cs* dosyalarını `RazorGenerate` öğesi öğeleri. Kullanım `RazorGenerateDependsOn` özelliği ek hedefler belirtmek için önce veya sonra bu hedef çalıştırabilirsiniz.
* `RazorCompile` &ndash; Oluşturulan derler *.cs* için bir Razor derleme dosyaları. Kullanım `RazorCompileDependsOn` önce veya sonra bu hedef çalıştırabilirsiniz ek hedefleri belirtmek için.

### <a name="runtime-compilation-of-razor-views"></a>Razor görünüm çalışma zamanı derlemesi

* Varsayılan olarak, Razor SDK'sı, çalışma zamanı derleme gerçekleştirmek için gereken başvuru derlemelerini yayımlama değil. Çalışma zamanı derleme sırasında uygulama modeli kullanır, bu derleme hataları sonuçları&mdash;Örneğin, uygulama yayımlandıktan sonra uygulamayı katıştırılmış görünümleri ya da değişiklikleri görünümler kullanır. Ayarlama `CopyRefAssembliesToPublishDirectory` için `true` başvuru bütünleştirilmiş kodları yayımlamaya devam etmek için.

* Bir web uygulaması için uygulamanızın hedeflediği olun `Microsoft.NET.Sdk.Web` SDK.
