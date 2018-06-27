---
title: ASP.NET Core dizin yapısı
author: guardrex
description: Yayımlanan ASP.NET Core uygulamaları dizin yapısı hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 8e2693397f826d0e9a36ff52aa1d1d623b31043d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960833"
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core dizin yapısı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core, yayımlanan uygulama dizini içinde *yayımlama*, uygulama dosyaları, yapılandırma dosyaları, statik varlıklar, paketleri ve çalışma zamanı oluşur (için [müstakil dağıtımları](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Uygulama türü | Dizin yapısı |
| -------- | ------------------- |
| [Framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Yayımlama&dagger;<ul><li>Günlükleri&dagger; (isteğe bağlı stdout günlükleri almak için gerekli olmadıkça)</li><li>Görünümler&dagger; (MVC uygulamaları; görünümleri önceden derlenmiş değil ise)</li><li>Sayfaları&dagger; (sayfaları önceden derlenmiş değil, MVC veya Razor sayfalarının uygulamaları;)</li><li>wwwroot&dagger;</li><li>*\.DLL dosyaları</li><li>\<derleme adı >. deps.json</li><li>\<derleme adı > .dll</li><li>\<derleme adı > .pdb</li><li>\<derleme adı >. PrecompiledViews.dll</li><li>\<derleme adı >. PrecompiledViews.pdb</li><li>\<derleme adı >. runtimeconfig.json</li><li>Web.config (IIS dağıtımlar)</li></ul></li></ul> |
| [Kendi içinde bulunan dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Yayımlama&dagger;<ul><li>Günlükleri&dagger; (isteğe bağlı stdout günlükleri almak için gerekli olmadıkça)</li><li>refs&dagger;</li><li>Görünümler&dagger; (MVC uygulamaları; görünümleri önceden derlenmiş değil ise)</li><li>Sayfaları&dagger; (sayfaları önceden derlenmiş değil, MVC veya Razor sayfalarının uygulamaları;)</li><li>wwwroot&dagger;</li><li>\*.dll dosyaları</li><li>\<derleme adı >. deps.json</li><li>\<derleme adı > .exe</li><li>\<derleme adı > .pdb</li><li>\<derleme adı >. PrecompiledViews.dll</li><li>\<derleme adı >. PrecompiledViews.pdb</li><li>\<derleme adı >. runtimeconfig.json</li><li>Web.config (IIS dağıtımlar)</li></ul></li></ul> |

&dagger;Bir dizini gösterir

*Yayımlama* directory temsil eden *içerik kök yolu*, olarak da bilinir *uygulama temel yolu*, dağıtım. Hangi adı verilir *yayımlama* Dizin sunucusundaki dağıtılan uygulamanın konumuna barındırılan uygulamasının fiziksel yolu sunucunun görür.

*Wwwroot* dizini, varsa, yalnızca içeren statik varlıklar.

Stdout *günlükleri* iki yaklaşımın şunlardan birini kullanarak dağıtım için dizin oluşturulabilir:

* Aşağıdakileri ekleyin `<Target>` proje dosyası öğesine:

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

   `<MakeDir>` Öğesi oluşturur boş bir *günlükleri* yayımlanan çıkış klasöründe. Öğesini kullanan `PublishDir` klasörü oluşturmak için hedef konum belirlemek için özellik. Web dağıtımı gibi birkaç dağıtım yöntemleri, dağıtım sırasında boş klasörler atlayın. `<WriteLinesToFile>` Öğesi oluşturur bir dosyada *günlükleri* sunucu klasörüne dağıtımını garanti klasör. Çalışan işlemi, hedef klasöre yazma erişimi yoksa, klasör oluşturma yine başarısız olabilir unutmayın.

* Fiziksel olarak oluşturma *günlükleri* dağıtımdaki sunucuya dizin.

Dağıtım dizini okuma/Yürütme izinleri gerektirir. *Günlükleri* dizin okuma/yazma izinleri gerektirir. Dosyaları yazılacağı ek dizinler okuma/yazma izinleri gerektirir.
