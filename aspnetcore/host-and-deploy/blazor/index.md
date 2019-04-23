---
title: Barındırma ve Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve Blazor uygulamaları dağıtmak nasıl keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 774fbb6fdaab14a015db4fb39de2e1ea73a1837b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982646"
---
# <a name="host-and-deploy-blazor"></a>Barındırma ve Blazor dağıtma

Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Uygulamalar ile yayın Yapılandırması'nda dağıtım için yayımlanır [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu. Yürütülen bir tümleşik geliştirme ortamı (IDE) işleyebilir `dotnet publish` komutunu otomatik olarak yerleşik yayımlama özelliklerini kullanarak, bunu el ile gerekli olmayabilir bu nedenle geliştirme bağlı olarak bir komut isteminden komutu yürütün Araçlar kullanılıyor.

```console
dotnet publish -c Release
```

`dotnet publish` Tetikleyiciler bir [geri](/dotnet/core/tools/dotnet-restore) proje bağımlılıklarının ve [yapılar](/dotnet/core/tools/dotnet-build) dağıtım için bir varlık oluşturmadan önce proje. Yapı işleminin bir parçası olarak, kullanılmayan yöntemleri ve derlemeleri, uygulama indirme boyutunu küçültmek ve yükleme süreleri için kaldırılır. Dağıtım oluşturulan */bin/yayın / {hedef çerçeve} / publish* klasör.

Varlıkları *yayımlama* klasör, web sunucusuna dağıtılır. Dağıtım işlemini kullanımda geliştirme araçları bağlı olarak otomatik veya el ile olabilir.

## <a name="deployment"></a>Dağıtım

Dağıtım yönergeleri için aşağıdaki konulara bakın:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
