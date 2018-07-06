---
uid: webhooks/diagnostics/debugging
title: ASP.NET hata ayıklama Web kancaları | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları hata ayıklamak nasıl.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801994"
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET hata ayıklama Web kancaları  

## <a name="debugging-in-azure"></a>Azure'da hata ayıklama

Web uygulamanızı Azure'da çalışırken hata ayıklama için öğreticiye bakın [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Kaynak ve simgeler ile hata ayıklama

Kendi kod hata ayıklama ek olarak, Microsoft ASP.NET WebHooks doğrudan ve hatta hata ayıklamak mümkündür tüm .NET. Bu, yerel olarak veya uzaktan hata ayıklama bağımsız olarak çalışır. İlk olarak, giderek kaynak ve simgeleri bulmak için Visual Studio yapılandırma **hata ayıklama** ardından **seçenekler ve ayarlar**. Bu gibi seçenekleri ayarlayın:

![Seçenekler ve ayarlar](_static/SourceSymbols.png)

Ardından bir bağlantı ekleyin [symbolsource.org](http://symbolsource.org) kaynak ve sembol karşıdan yüklemek için. Git **sembolleri** sekmesinde menüsünden izleyebilirim ve bir simge konumu olarak aşağıdakileri ekleyin:

```
http://srv.symbolsource.org/pdb/Public
```

Ayrıca, önbellek dizini kısa bir ad olduğundan emin olun; Aksi takdirde dosya adları değil yüklenecek semboller neden olacak uzun elde edebilirsiniz. Bir örnek yoludur:

```
C:\SymCache
```

Ayarları şuna benzer görünmelidir:

![Seçenekleri sembol dosyası konumu örneği](_static/SymSource.png)
