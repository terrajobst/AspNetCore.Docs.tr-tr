---
title: Araçlar ve indirmeler - ASP.NET Core ve Azure ile DevOps
author: CamSoper
description: Araçları ve ASP.NET Core ve Azure ile DevOps için gerekli yüklemeler.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 9c1042dd48b9167209b46e97a09e011b80e2609c
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511151"
---
# <a name="tools-and-downloads"></a>Araçlar ve indirmeler

Azure 'da [Azure Portal](https://portal.azure.com), [azure CLI](/cli/azure/), [Azure PowerShell](/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash)ve Visual Studio gibi kaynakları sağlamak ve yönetmek için çeşitli arabirimler vardır. Bu kılavuz, minimalist bir yaklaşım ve Azure Cloud Shell'i mümkün olduğunca azaltmak gerekli adımlar kullanır. Ancak, Azure portalında bazı kısımları için kullanılmalıdır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki abonelikler gereklidir:

* Azure &mdash; hesabınız yoksa [ücretsiz deneme sürümü alın](https://azure.microsoft.com/free/).
* Azure DevOps Services &mdash; Azure DevOps aboneliğiniz ve kuruluşunuz Bölüm 4 ' te oluşturulmuştur.
* GitHub &mdash; bir hesabınız yoksa [ücretsiz kaydolun](https://github.com/join).

Aşağıdaki araçları gereklidir:

* [Git](https://git-scm.com/downloads) &mdash; Bu kılavuz için git 'in temel olarak anlaşılmasına önerilir. [Git belgelerini](https://git-scm.com/doc)gözden geçirin, özellikle [Git uzak](https://git-scm.com/docs/git-remote) ve [Git Push](https://git-scm.com/docs/git-push).
* Örnek uygulamayı derlemek ve çalıştırmak için [.NET Core SDK](https://dotnet.microsoft.com/download/) &mdash; sürüm 2.1.300 veya üzeri gereklidir. Visual Studio, **.NET Core platformlar arası geliştirme** iş yüküne yüklenmişse, .NET Core SDK zaten yüklüdür.

    .NET Core SDK'yı yüklemenizi doğrulayın. Bir komut kabuğunu açın ve aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a>Önerilen Araçlar (yalnızca Windows)

* [Visual Studio](https://visualstudio.microsoft.com)'Nun sağlam Azure Araçları, bu kılavuzda açıklanan işlevselliğin çoğu IÇIN bir GUI sağlar. Ücretsiz Visual Studio Community sürümü dahil olmak üzere herhangi bir Visual Studio sürümünü çalışır. Öğreticiler, geliştirme, dağıtma ve DevOps hem ile hem de Visual Studio olmadan göstermek için yazılır.

  Visual Studio 'Nun aşağıdaki [iş yüklerinin](/visualstudio/install/modify-visual-studio) yüklü olduğunu doğrulayın:

  * ASP.NET ve web geliştirme
  * Azure geliştirme
  * .NET Core çoklu platform geliştirme
