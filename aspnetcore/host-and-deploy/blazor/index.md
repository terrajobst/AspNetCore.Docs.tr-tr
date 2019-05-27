---
title: Barındırma ve Blazor dağıtma
author: guardrex
description: Ana bilgisayar ve Blazor uygulamaları dağıtmak nasıl keşfedin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5def0356d13975211dd234f6a6a9f5a993d003b7
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223175"
---
# <a name="host-and-deploy-blazor"></a>Barındırma ve Blazor dağıtma

Tarafından [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), ve [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Uygulamalar, yayın Yapılandırması'nda dağıtım için yayımlanır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Seçin **derleme** > **yayımlama {uygulama}** gezinti çubuğundan.
1. Seçin *hedef yayımlama*. Yerel olarak yayımlamak için seçin **klasör**.
1. Varsayılan konumu kabul **bir klasör seçin** alanına veya farklı bir konum belirtin. **Yayımla** düğmesini seçin.


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

Kullanım [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) komutu bir sürüm yapılandırması ile uygulama yayımlama:

```console
dotnet publish -c Release
```

---

Uygulama Tetikleyicileri yayımlama bir [geri](/dotnet/core/tools/dotnet-restore) proje bağımlılıklarının ve [yapılar](/dotnet/core/tools/dotnet-build) dağıtım için bir varlık oluşturmadan önce proje. Yapı işleminin bir parçası olarak, kullanılmayan yöntemleri ve derlemeleri, uygulama indirme boyutunu küçültmek ve yükleme süreleri için kaldırılır.

Bir Blazor istemci-tarafı uygulaması yayımlanan */bin/yayın / {hedef çerçeve} / dist* klasör. İçin yayımlanan bir Blazor sunucu tarafı uygulamayı */bin/yayın / {hedef çerçeve} / publish* klasör.

Varlıkları klasöründe, web sunucusuna dağıtılır. Dağıtım işlemini kullanımda geliştirme araçları bağlı olarak otomatik veya el ile olabilir.

## <a name="deployment"></a>Dağıtım

Dağıtım yönergeleri için aşağıdaki konulara bakın:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Azure Depolama'yı barındıran sunucusuz Blazor

Gelen istemci tarafı uygulamalar Blazor sunulduğundan [Azure depolama](https://azure.microsoft.com/services/storage/) statik içeriği doğrudan bir depolama kapsayıcısı olarak.

Daha fazla bilgi için [konak ve Blazor istemci-tarafı (tek başına dağıtımı) dağıtın: Azure depolama](xref:host-and-deploy/blazor/client-side#azure-storage).
