---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Daha kolay ve MVC kullanmaktan daha üretken ASP.NET Core Razor sayfalarında kodlama sayfa odaklı senaryoları nasıl kolaylaştırdığını öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: 5217f17045a0fec09a8a67b4c9f132b1cebeaf1e
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2018
ms.locfileid: "35612885"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [](~/includes/2.1-SDK.md)] İçeren `Microsoft.NET.Sdk.Razor` MSBuild SDK'sı (Razor SDK). Razor SDK:

* Derleme, paketleme ve içeren projeleri yayımlama çevresinde deneyimi standartlaştıran [Razor](xref:mvc/views/razor) ASP.NET Core MVC tabanlı projelerin dosyalarını.
* Bir dizi önceden tanımlanmış hedefleri, özellikleri ve Razor dosyaları derlenmesini özelleştirmeye izin ver öğelerini içerir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>SDK Razor kullanma

Çoğu web uygulamaları açıkça Razor SDK'sı başvurusu gerek yoktur. 

Razor görünümleri veya Razor sayfalarının içeren sınıf kitaplıkları oluşturmak için Razor SDK'yı kullanmak için:

* Kullanım `Microsoft.NET.Sdk.Razor` yerine `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* Genellikle bir paketi başvurusu `Microsoft.AspNetCore.Mvc` oluşturmak ve Razor sayfalarını ve Razor görünümleri derlemek için gereken ek bağımlılıklar getirmek için gereklidir. En azından, projenizin paket referanslarını eklemesi gerekir:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Önceki paketleri dahil edilen `Microsoft.AspNetCore.Mvc`. Aşağıdaki biçimlendirmede temel gösterir *.csproj* Razor SDK'sı bir ASP.NET Core Razor sayfalarının uygulama için Razor dosyaları oluşturmak için kullanılan dosya:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Özellikler

Aşağıdaki özellikler proje derlemesi bir parçası olarak Razor'ın SDK davranışını denetleyen:

* `RazorCompileOnBuild` : Ne zaman `true`derler ve proje oluşturmanın bir parçası olarak Razor derleme yayar. Varsayılan olarak `true`.
* `RazorCompileOnPublish` : Ne zaman `true`derler ve Razor derleme projesi yayımlama bir parçası olarak yayar. Varsayılan olarak `true`.

Aşağıdaki özellikleri ve öğeleri ve Razor SDK çıkış girişleri yapılandırmak için kullanılır:

| Öğeler                                         | Açıklama                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Madde öğeleri (*.cshtml* dosyaları) girişleri kod oluşturma hedefleri olan. |
| RazorCompile                                  | Razor derleme hedefleri girdiler öğesi öğeleri (.cs dosyaları). Razor derlemeye derlenecek ek dosyaları belirtmek için bu ItemGroup kullanın. |
| RazorTargetAssemblyAttribute                  | Kod için kullanılan öğesi öğeleri Razor derleme için öznitelikler oluşturun. Örneğin:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Oluşturulan Razor derleme katıştırılmış kaynakları olarak eklenen öğesi öğeleri |

| Özellik                                      | Açıklama                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Razor tarafından üretilen derleme dosya adı (uzantısı olmadan). | 
| RazorOutputPath                               | Razor çıktı dizini.                                      |
| RazorCompileToolset                           | Razor derleme oluşturmak için kullanılan araç takımı belirlemek için kullanılır. Geçerli değerler `Implicit`,, ve `PrecompilationTool`. |
| EnableDefaultContentItems                     | Zaman `true`, belirli dosya türlerini içerir *.cshtml* içeriği projedeki dosyaları. Microsoft.NET.Sdk.Web başvurulduğunda altındaki tüm dosyaları da içerir *wwwroot*ve yapılandırma dosyaları.         |
| EnableDefaultRazorGenerateItems               | Zaman `true`, içeren *.cshtml* dosyaları buradan `Content` öğeler `RazorGenerate` öğeleri. |
| GenerateRazorTargetAssemblyInfo               | Zaman `true`, oluşturan bir *.cs* tarafından belirtilen öznitelikler içeren bir dosya `RazorAssemblyAttribute` ve derleme çıktısında içerir. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Zaman `true`, varsayılan bir derleme özniteliklerini ekler `RazorAssemblyAttribute`. |
| CopyRazorGenerateFilesToPublishDirectory       | Zaman `true`, RazorGenerate öğeleri kopyalar (*.cshtml*) dosyalarını Yayımla dizinine kopyalayın. Derleme zamanı veya yayımlama zaman derlemede katılırsanız genellikle Razor dosyaları yayımlanan uygulama için gerekli değildir. Varsayılan olarak `false`. |
| CopyRefAssembliesToPublishDirectory            | Zaman `true`, başvuru derleme öğeleri Yayımla dizinine kopyalayın. Razor derleme derleme zamanında veya yayımlama saatte oluşursa genellikle başvuru derlemeleri yayımlanan uygulama için gerekli değildir. Kümesine `true`, kullanıcılarınızın yayımlanan uygulamanıza çalışma zamanı derlemesi, örneğin, çalışma zamanında cshtml dosyaları değiştirir veya kullanan katıştırılmış görünümleri gerektiriyorsa. Varsayılan olarak `false`. |
| IncludeRazorContentInPack                      | Zaman `true`, tüm Razor içerik öğeleri (*.cshtml* dosyaları) oluşturulan NuGet paketi eklenmek üzere işaretlenir. Varsayılan olarak `false`. |
| EmbedRazorGenerateSources | Zaman `true`, RazorGenerate ekler (*.cshtml*) öğeleri oluşturulan Razor derleme ekli dosyaları olarak. Varsayılan olarak `false`. |
| UseRazorBuildServer                           | Zaman `true`, kalıcı derleme sunucu sürecinde kod oluşturma iş boşaltmak için kullanır. Varsayılan olarak değerine `UseSharedCompilation`. |

### <a name="targets"></a>Hedefler
Razor SDK'sı iki birincil hedefleri tanımlar:

* `RazorGenerate` -Kod oluşturur *.cs* RazorGenerate dosyalardan öğeleri öğesi. Kullanım `RazorGenerateDependsOn` özelliği ek hedefler belirtmek için önce veya sonra bu hedefi çalıştırabilirsiniz.
* `RazorCompile` -Derlerken oluşturulan *.cs* bir Razor derleme dosyaları. Kullanım `RazorCompileDependsOn` önce veya sonra bu hedefi çalıştırabilirsiniz ek hedefleri belirtmek için.

### <a name="runtime-compilation-of-razor-views"></a>Razor görünümlerini çalışma zamanı derlemesi

* Varsayılan olarak, çalışma zamanı derlemesi gerçekleştirmek için gereken başvuru derlemeleri Razor SDK yayımlama değil. Çalışma zamanı derlemesi uygulama modeli kullanır bu sonuçlara derleme hataları&mdash;Örneğin, uygulama yayımlandıktan sonra uygulama katıştırılmış görünümleri veya değişiklikleri görünümleri kullanır. Ayarlama `CopyRefAssembliesToPublishDirectory` için `true` başvuru derlemeleri yayımlamaya devam etmek için.

* Web uygulamaları için uygulamanızı hedefleme olun `Microsoft.NET.Sdk.Web` SDK.
