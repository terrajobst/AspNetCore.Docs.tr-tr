---
title: "ASP.NET Core dizin yapısı"
author: guardrex
description: "Yayımlanan ASP.NET Core uygulamaları dizin yapısını bakın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 55e1e0dac32609446243098dbb4a4373f06b4212
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Yayımlanan ASP.NET Core uygulamaları dizin yapısı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core, uygulama dizini içinde *yayımlama*, uygulama dosyaları, yapılandırma dosyaları, statik varlıklar, paketleri ve Çalışma Zamanı Modülü (kendi içinde bulunan uygulamalar) oluşur.

| Uygulama türü                       | Dizin yapısı |
| ------------------------------ | ------------------- |
| Framework bağımlı dağıtım | <ul><li>Yayımlama\*<ul><li>Günlükleri\* (publishOptions içinde yer alan varsa)</li><li>refs\*</li><li>Çalışma zamanları\*</li><li>Görünümler\* (publishOptions içinde yer alan varsa)</li><li>wwwroot\* (publishOptions içinde yer alan varsa)</li><li>.dll dosyaları</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.dll</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.pdb</li><li>myapp.runtimeconfig.json</li><li>(publishOptions içinde yer alan varsa) web.config</li></ul></li></ul> |
| Kendi içinde bulunan dağıtım      | <ul><li>Yayımlama\*<ul><li>Günlükleri\* (publishOptions içinde yer alan varsa)</li><li>refs\*</li><li>Görünümler\* (publishOptions içinde yer alan varsa)</li><li>wwwroot\* (publishOptions içinde yer alan varsa)</li><li>.dll dosyaları</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.dll</li><li>Uygulamam. (Razor görünümleri önceden derleme varsa) PrecompiledViews.pdb</li><li>myapp.runtimeconfig.json</li><li>(publishOptions içinde yer alan varsa) web.config</li></ul></li></ul> |
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
