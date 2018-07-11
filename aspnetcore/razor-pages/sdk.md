---
title: ASP.NET Core Razor SDK'sı
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/sdk
ms.openlocfilehash: 4dd48b13272ed847ff83e8826e10678d732b53f9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38215891"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK'sı

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [](~/includes/2.1-SDK.md)] İçerir `Microsoft.NET.Sdk.Razor` MSBuild SDK'sı (Razor SDK). Razor SDK:

* Oluşturma, paketleme ve içeren proje yayımlama deneyimi standartlaştırır [Razor](xref:mvc/views/razor) dosyaları ASP.NET Core MVC tabanlı projeler için.
* Bir dizi önceden tanımlanmış hedefleri, özellikler ve Razor dosyaları derleme özelleştirme izin veren öğeleri içerir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Razor SDK'sını kullanma

Çoğu web uygulaması, açıkça Razor SDK'sına başvuran gerekmez. 

Razor görünümleri ya da Razor sayfaları içeren sınıf kitaplıkları oluşturmak için Razor SDK'sını kullanmak için:

* Kullanım `Microsoft.NET.Sdk.Razor` yerine `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Genellikle bir paket başvurusu `Microsoft.AspNetCore.Mvc` oluşturmak ve Razor sayfaları ve Razor görünümleri derlemek için gereken ek bağımlılıklar getirmek için gereklidir. En azından, paket başvuruları eklemek, projenizin gerekir:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Yukarıdaki paketleri dahil `Microsoft.AspNetCore.Mvc`. Aşağıdaki biçimlendirme gösteren temel bir *.csproj* Razor SDK'sı bir ASP.NET Core Razor sayfalar uygulama için Razor dosyaları oluşturmak için kullanılan dosya:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Özellikler

Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışı denetler:

* `RazorCompileOnBuild` : Ne zaman `true`, derler ve Razor derleme proje oluşturma işleminin parçası olarak yayar. Varsayılan olarak `true`.
* `RazorCompileOnPublish` : Ne zaman `true`, derler ve Razor derleme proje yayımlama işleminin parçası olarak yayar. Varsayılan olarak `true`.

Aşağıdaki özellikleri ve öğeleri ve Razor SDK çıkış Girişleri'ı yapılandırmak için kullanılır:

| Öğeler                                         | Açıklama                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Öğe öğeleri (*.cshtml* dosyaları) girişleri kod oluşturma hedefleri olan. |
| RazorCompile                                  | Razor derleme hedefleri girişleri öğesi öğeleri (.cs dosyaları). Bu ItemGroup Razor bütünleştirilmiş kod içine derlenmiş için ek dosyaları belirtmek için kullanın. |
| RazorTargetAssemblyAttribute                  | Kod için kullanılan öğeler için Razor derleme öznitelikleri oluşturur. Örneğin:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Oluşturulan Razor derlemesine katıştırılmış kaynakları olarak eklenen öğeler |

| Özellik                                      | Açıklama                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Razor tarafından üretilen derleme dosya adı (uzantısı olmadan). | 
| RazorOutputPath                               | Razor çıkış dizini.                                      |
| RazorCompileToolset                           | Razor derlemesi oluşturmak için kullanılan araç kümesini belirlemek için kullanılır. Geçerli değerler `Implicit`,, ve `PrecompilationTool`. |
| EnableDefaultContentItems                     | Zaman `true`, aşağıdakiler gibi belirli dosya türlerini içerir *.cshtml* içerik olarak proje dosyaları. Microsoft.NET.Sdk.Web başvurulduğunda altındaki tüm dosyaları da içerir *wwwroot*ve yapılandırma dosyaları.         |
| EnableDefaultRazorGenerateItems               | Zaman `true`, içeren *.cshtml* dosyalarını `Content` öğeler `RazorGenerate` öğeleri. |
| GenerateRazorTargetAssemblyInfo               | Zaman `true`, oluşturur bir *.cs* tarafından belirtilen öznitelikler içeren dosya `RazorAssemblyAttribute` ve derleme çıktısında içerir. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Zaman `true`, derleme öznitelikleri için varsayılan ayarı ekler `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Zaman `true`, RazorGenerate öğeleri kopyalar (*.cshtml*) dosyalarını yayımlama dizinine kopyalayın. Derleme zamanı veya yayımlama zamanı derleme katıldıkları, genellikle Razor dosyaları bir yayımlanan uygulama için gerekli değildir. Varsayılan olarak `false`. |
| CopyRefAssembliesToPublishDirectory            | Zaman `true`, başvuru bütünleştirilmiş kod öğeleri Yayımla dizinine kopyalayın. Razor derleme, derleme zamanı veya yayımlama zamanı oluşursa genellikle başvuru bütünleştirilmiş kodları yayınlanmış bir uygulama için gerekli değildir. Kümesine `true`, yayımlanan uygulamanızın çalışma zamanı derlemesi, örneğin, çalışma zamanında cshtml dosyaları değiştirir veya kullanan katıştırılmış görünümleri gerektirir. Varsayılan olarak `false`. |
| IncludeRazorContentInPack                      | Zaman `true`, tüm Razor içerik öğeleri (*.cshtml* dosyaları) üretilen NuGet paketini eklenmek üzere işaretlenir. Varsayılan olarak `false`. |
| EmbedRazorGenerateSources | Zaman `true`, RazorGenerate ekler (*.cshtml*) öğeleri olarak oluşturulmuş Razor derlemesine katıştırılmış dosyaları. Varsayılan olarak `false`. |
| UseRazorBuildServer                           | Zaman `true`, kod oluşturma iş yüklerini boşaltmak üzere bir sürekli derleme sunucu işlemi kullanır. Varsayılan olarak, değeri olarak `UseSharedCompilation`. |

### <a name="targets"></a>Hedefler
Razor SDK'sı iki birincil hedefleri tanımlar:

* `RazorGenerate` -Kod oluşturur *.cs* RazorGenerate dosyalarından öğe öğeleri. Kullanım `RazorGenerateDependsOn` özelliği ek hedefler belirtmek için önce veya sonra bu hedef çalıştırabilirsiniz.
* `RazorCompile` -Derler oluşturulan *.cs* için bir Razor derleme dosyaları. Kullanım `RazorCompileDependsOn` önce veya sonra bu hedef çalıştırabilirsiniz ek hedefleri belirtmek için.

### <a name="runtime-compilation-of-razor-views"></a>Razor görünüm çalışma zamanı derlemesi

* Varsayılan olarak, Razor SDK'sı, çalışma zamanı derleme gerçekleştirmek için gereken başvuru derlemelerini yayımlama değil. Çalışma zamanı derleme sırasında uygulama modeli kullanır, bu derleme hataları sonuçları&mdash;Örneğin, uygulama yayımlandıktan sonra uygulamayı katıştırılmış görünümleri ya da değişiklikleri görünümler kullanır. Ayarlama `CopyRefAssembliesToPublishDirectory` için `true` başvuru bütünleştirilmiş kodları yayımlamaya devam etmek için.

* Web uygulamaları için uygulamanızın hedeflediği olun `Microsoft.NET.Sdk.Web` SDK.
