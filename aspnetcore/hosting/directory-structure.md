---
title: "ASP.NET Core dizin yapısı"
author: guardrex
description: "Yayımlanan ASP.NET Core uygulamaları dizin yapısını."
keywords: "ASP.NET Core, dizin yapısı"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Yayımlanan ASP.NET Core uygulamaları dizin yapısı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core, uygulama dizini içinde *yayımlama*, uygulama dosyaları, yapılandırma dosyaları, statik varlıklar, paketleri ve Çalışma Zamanı Modülü (kendi içinde bulunan uygulamalar) oluşur. ASP.NET, web kök dizini tüm uygulama nerede yaşıyor önceki sürümlerinde aynı dizin yapısını budur.

| Uygulama türü | Dizin yapısı |
| --- | --- |
| Framework bağımlı dağıtım | <ul><li>Yayımlama\*<ul><li>Günlükleri\* (publishOptions içinde yer alan varsa)</li><li>refs\*</li><li>Çalışma zamanları\*</li><li>Görünümler\* (publishOptions içinde yer alan varsa)</li><li>wwwroot\* (publishOptions içinde yer alan varsa)</li><li>.dll dosyaları</li><li>MyApp.deps.JSON</li><li>MyApp.dll</li><li>MyApp.pdb</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.dll</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.pdb</li><li>MyApp.runtimeconfig.JSON</li><li>(publishOptions içinde yer alan varsa) web.config</li></ul></li></ul> |
| Kendi içinde bulunan dağıtım | <ul><li>Yayımlama\*<ul><li>Günlükleri\* (publishOptions içinde yer alan varsa)</li><li>refs\*</li><li>Görünümler\* (publishOptions içinde yer alan varsa)</li><li>wwwroot\* (publishOptions içinde yer alan varsa)</li><li>.dll dosyaları</li><li>MyApp.deps.JSON</li><li>MyApp.exe</li><li>MyApp.pdb</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.dll</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.pdb</li><li>MyApp.runtimeconfig.JSON</li><li>(publishOptions içinde yer alan varsa) web.config</li></ul></li></ul> |
\*Bir dizini gösterir

İçeriğini *yayımlama* directory temsil eden *içerik kök yolu*, olarak da bilinir *uygulama temel yolu*, dağıtım. Hangi adı verilir *yayımlama* dağıtım dizininde, konumunda barındırılan uygulama sunucunun fiziksel yolu görür. *Wwwroot* dizini, varsa, yalnızca içeren statik varlıklar. *Günlükleri* dizin dahil edilebilir dağıtımda projede oluşturma ve ekleme `<Target>` aşağıda gösterildiği, *.csproj* dosya veya dizin fiziksel olarak oluşturma Sunucu.

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

Dağıtım dizini okuma/Yürütme izinleri gerektirir ancak *günlükleri* dizin okuma/yazma izinleri gerektirir. Varlıklar yazılacağı ek dizinler okuma/yazma izinleri gerektirir.
