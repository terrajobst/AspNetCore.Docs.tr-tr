---
uid: webhooks/diagnostics/debugging
title: "ASP.NET hata ayıklama Kancalarını | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Web Kancalarını hata ayıklamak nasıl."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET Web Kancalarını hata ayıklama  

## <a name="debugging-in-azure"></a>Azure'da hata ayıklama

Web uygulamanızı Azure'da çalıştırılırken hata ayıklamak için lütfen öğretici bakın [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Kaynak ve simgeler ile hata ayıklama

Kendi kodunuzu hata ayıklama ek olarak, Microsoft ASP.NET WebHooks doğrudan ve hatta hata ayıklamak mümkündür tüm .NET. Bu, olup, yerel olarak veya uzaktan hata ayıklama bağımsız olarak çalışır. İlk olarak, kaynak ve simgeleri giderek bulmak için Visual Studio'yu yapılandırma **hata ayıklama** ve ardından **seçenekleri ve ayarları**. Bu gibi seçenekleri ayarlayın:

![Seçenekler ve ayarlar](_static/SourceSymbols.png)

Bağlantı Ekle [symbolsource.org](http://symbolsource.org) kaynak ve simgeleri indirme. Git **simgeleri** yukarıdaki menüsünün sekmesini ve bir simgenin konumu olarak aşağıdakileri ekleyin:

```
http://srv.symbolsource.org/pdb/Public
```

Ayrıca, önbellek dizini kısa bir ad olduğundan emin olun; Aksi takdirde dosya adları yüklenmemesi simgeler neden olacak çok uzun elde edebilirsiniz. Bir örnek yolu şöyledir:

```
C:\SymCache
```

Ayarları şuna benzer görünmelidir:

![Seçenekler simgesi dosyası konumu örneği](_static/SymSource.png)
