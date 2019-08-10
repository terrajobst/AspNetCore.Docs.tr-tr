---
title: ASP.NET Core Blazor barındırma ve dağıtma
author: guardrex
description: Blazor uygulamalarının nasıl barındırılacağını ve dağıtılacağını öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: d18abbf33c71dca5130bfc6b503b46c1d5bce537
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913933"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>ASP.NET Core Blazor barındırma ve dağıtma

, [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)ve [Daniel Roth](https://github.com/danroth27) tarafından

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Uygulamalar yayın yapılandırmasında dağıtım için yayımlanır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Gezinti çubuğundan **Build** > **Publish {APPLICATION}** öğesini seçin.
1. *Yayımla hedefini*seçin. Yerel olarak yayımlamak için **klasör**' ü seçin.
1. **Klasör seçin** alanında varsayılan konumu kabul edin veya farklı bir konum belirtin. **Yayımla** düğmesini seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uygulamayı bir sürüm yapılandırmasıyla yayımlamak için [DotNet Publish](/dotnet/core/tools/dotnet-publish) komutunu kullanın:

```console
dotnet publish -c Release
```

---

Uygulamanın yayımlanması, projenin bağımlılıklarını [geri yüklemeyi](/dotnet/core/tools/dotnet-restore) tetikler ve dağıtım için varlıkları oluşturmadan önce projeyi [oluşturur](/dotnet/core/tools/dotnet-build) . Yapı işleminin bir parçası olarak, uygulama indirme boyutunu ve yükleme sürelerini azaltmak için kullanılmayan Yöntemler ve derlemeler kaldırılır.

Blazor istemci tarafı uygulaması */BIN/Release/{Target Framework}/publish/{ASSEMBLY Name}/Dist* klasörüne yayımlanır. Blazor sunucu tarafı bir uygulama */BIN/Release/{Target Framework}/Publish* klasörüne yayımlanır.

Klasördeki varlıklar Web sunucusuna dağıtılır. Dağıtım, kullanımdaki geliştirme araçlarına bağlı olarak el ile veya otomatik bir süreç olabilir.

## <a name="deployment"></a>Dağıtım

Dağıtım Kılavuzu için aşağıdaki konulara bakın:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Azure depolama ile Blazor sunucusuz barındırma

Blazor istemci tarafı uygulamalar, doğrudan bir depolama kapsayıcısından statik içerik olarak [Azure depolama](https://azure.microsoft.com/services/storage/) 'dan sunulabilir.

Daha fazla bilgi için bkz [. konak ve dağıtım ASP.NET Core Blazor istemci tarafı (tek başına dağıtım): Azure depolama](xref:host-and-deploy/blazor/client-side#azure-storage).
