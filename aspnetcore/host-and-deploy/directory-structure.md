---
title: ASP.NET Core dizin yapısı
author: rick-anderson
description: Dizin yapısı, yayımlanmış ASP.NET Core uygulamaları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f7d6feec9961b7f6720d30d457fae5dcb6b34d6c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664110"
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core dizin yapısı

::: moniker range=">= aspnetcore-3.0"

*Yayımla* dizini, uygulamanın [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutu tarafından üretilen dağıtılabilir varlıkları içerir. Dizin şunları içerir:

* Uygulama dosyaları
* Yapılandırma dosyaları
* Statik varlıklar
* Paketler
* Çalışma zamanı (yalnızca[kendi içindeki dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) )

| Uygulama türü | Dizin yapısı |
| -------- | ------------------- |
| [Çerçeveye bağımlı yürütülebilir (FDE)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>&dagger; Yayımla<ul><li>&dagger; MVC uygulamalarını görüntüler; Görünümler önceden derlenmiş değilse</li><li>Sayfalar önceden derlenmiş değilse MVC veya Razor Pages uygulamalar&dagger;.</li><li>Wwwroot&dagger;</li><li>\*. dll dosyaları</li><li>{ASSEMBLY NAME}. Deps. JSON</li><li>{Bütünleştirilmiş kod adı}. dll</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı} {. Windows üzerinde uzantı} *. exe* uzantısı, MacOS veya Linux üzerinde uzantı yok</li><li>{Bütünleştirilmiş kod adı}. pdb</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. dll</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. pdb</li><li>{ASSEMBLY NAME}. runtimeconfig. JSON</li><li>Web. config (IIS dağıtımları)</li><li>createdump ([Linux createdump yardımcı programı](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))</li><li>\*. so (Linux paylaşılan nesne kitaplığı)</li><li>\*. a (macOS Arşivi)</li><li>\*. dylib (macOS dinamik kitaplığı)</li></ul></li></ul> |
| [Kendi içinde dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>&dagger; Yayımla<ul><li>Görünümler&dagger; MVC uygulamalarını önceden derlenmiş değilse görüntüler</li><li>Sayfalar önceden derlenmiş değilse MVC veya Razor Pages uygulamalar&dagger;.</li><li>Wwwroot&dagger;</li><li>\*. dll dosyaları</li><li>{ASSEMBLY NAME}. Deps. JSON</li><li>{Bütünleştirilmiş kod adı}. dll</li><li>{Bütünleştirilmiş kod adı}. exe</li><li>{Bütünleştirilmiş kod adı}. pdb</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. dll</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. pdb</li><li>{ASSEMBLY NAME}. runtimeconfig. JSON</li><li>Web. config (IIS dağıtımları)</li></ul></li></ul> |

&dagger;bir dizini belirtir

*Yayımla* dizini, dağıtımın *uygulama temel yolu*olarak da adlandırılan *içerik kök yolunu*temsil eder. Sunucuda dağıtılan uygulamanın *Yayımlama* dizinine hangi ad verildiğinde, konumu sunucunun barındırılan uygulamanın fiziksel yolu olarak görev yapar.

Varsa, *Wwwroot* dizini yalnızca statik varlıkları içerir.

## <a name="additional-resources"></a>Ek kaynaklar

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [.NET Core uygulama dağıtımı](/dotnet/core/deploying/)
* [Hedef çerçeveler](/dotnet/standard/frameworks)
* [.NET Core RID kataloğu](/dotnet/core/rid-catalog)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Yayımla* dizini, uygulamanın [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutu tarafından üretilen dağıtılabilir varlıkları içerir. Dizin şunları içerir:

* Uygulama dosyaları
* Yapılandırma dosyaları
* Statik varlıklar
* Paketler
* Çalışma zamanı (yalnızca[kendi içindeki dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) )

| Uygulama türü | Dizin yapısı |
| -------- | ------------------- |
| [Çerçeveye bağımlı yürütülebilir (FDE)](/dotnet/core/deploying/#framework-dependent-executables-fde) | <ul><li>&dagger; Yayımla<ul><li>&dagger; MVC uygulamalarını görüntüler; Görünümler önceden derlenmiş değilse</li><li>Sayfalar önceden derlenmiş değilse MVC veya Razor Pages uygulamalar&dagger;.</li><li>Wwwroot&dagger;</li><li>*. dll dosyaları</li><li>{Assembly Name}. Deps. json</li><li>{Assembly Name}. dll</li><li>{Assembly Name} {. UZANTı} *. exe* uzantısı Windows üzerinde, MacOS veya Linux üzerinde hiçbir uzantı</li><li>{Assembly Name}. pdb</li><li>{BÜTÜNLEŞTIRILMIŞ kod adı}. Views. dll <li>{bütünleştirilmiş kod adı}</li>. Görünümler. pdb</li><li>{ASSEMBLY NAME}. runtimeconfig. JSON</li><li>Web. config (IIS dağıtımları)</li><li>createdump ([Linux createdump Utility](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/xplat-minidump-generation.md#configurationpolicy))* </li><li>. bu (Linux paylaşılan nesne kitaplığı)</li><li>*. a (MacOS Arşivi)</li><li>* . dylib (MacOS dinamik kitaplığı)</li></ul></li></ul> |
| [Kendi içinde dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>&dagger; Yayımla<ul><li>Görünümler&dagger; MVC uygulamalarını önceden derlenmiş değilse görüntüler</li><li>Sayfalar önceden derlenmiş değilse MVC veya Razor Pages uygulamalar&dagger;.</li><li>Wwwroot&dagger;</li><li>*. dll dosyaları</li><li>{ASSEMBLY NAME}. Deps. JSON</li><li>{Bütünleştirilmiş kod adı}. dll</li><li>{Bütünleştirilmiş kod adı}. exe</li><li>{Bütünleştirilmiş kod adı}. pdb</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. dll</li><li>{BÜTÜNLEŞTIRILMIŞ KOD ADı}. Views. pdb</li><li>{ASSEMBLY NAME}. runtimeconfig. JSON</li><li>Web. config (IIS dağıtımları)</li></ul></li></ul> |

&dagger;bir dizini belirtir

*Yayımla* dizini, dağıtımın *uygulama temel yolu*olarak da adlandırılan *içerik kök yolunu*temsil eder. Sunucuda dağıtılan uygulamanın *Yayımlama* dizinine hangi ad verildiğinde, konumu sunucunun barındırılan uygulamanın fiziksel yolu olarak görev yapar.

Varsa, *Wwwroot* dizini yalnızca statik varlıkları içerir.

*Günlük* klasörü oluşturma, [ASP.NET Core modülü gelişmiş hata ayıklama günlüğü](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs)için yararlıdır. `<handlerSetting>` değere verilen yoldaki klasörler, modül tarafından otomatik olarak oluşturulmaz ve modülün hata ayıklama günlüğünü yazmasına izin vermek için dağıtımda önceden var olmalıdır.

Aşağıdaki iki yaklaşımdan birini kullanarak dağıtım için bir *günlük* dizini oluşturulabilir:

* Aşağıdaki `<Target>` öğesini proje dosyasına ekleyin:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   `<MakeDir>` öğesi yayımlanan çıktıda boş bir *Günlükler* klasörü oluşturur. Öğesi, klasörü oluşturmak için hedef konumu belirlemede `PublishDir` özelliğini kullanır. Web Dağıtımı gibi birkaç dağıtım yöntemi, dağıtım sırasında boş klasörleri atlar. `<WriteLinesToFile>` öğesi *Günlükler* klasöründe, klasörün sunucuya dağıtımını garanti eden bir dosya oluşturur. Çalışan işleminin hedef klasöre yazma erişimi yoksa, bu yaklaşımı kullanarak klasör oluşturma işlemi başarısız olur.

* Dağıtımdaki sunucuda *günlük* dizinini fiziksel olarak oluşturun.

Dağıtım dizini için okuma/yürütme izinleri gerekir. *Günlükler* dizini için okuma/yazma izinleri gerekir. Dosyaların yazıldığı ek dizinler okuma/yazma izinleri gerektirir.

## <a name="additional-resources"></a>Ek kaynaklar

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [.NET Core uygulama dağıtımı](/dotnet/core/deploying/)
* [Hedef çerçeveler](/dotnet/standard/frameworks)
* [.NET Core RID kataloğu](/dotnet/core/rid-catalog)

::: moniker-end
