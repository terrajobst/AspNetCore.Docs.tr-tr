---
title: Barındırma ve Razor bileşenleri ve Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve Razor bileşenleri ve Blazor uygulamaları dağıtmak nasıl keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59069779"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a>Barındırma ve Razor bileşenleri ve Blazor dağıtma

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

* [İstemci tarafı Blazor](xref:host-and-deploy/razor-components-blazor/blazor)
* [Sunucu tarafı ASP.NET Core Razor bileşenleri](xref:host-and-deploy/razor-components-blazor/razor-components)
