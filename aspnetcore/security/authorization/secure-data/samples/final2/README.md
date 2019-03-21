---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210112"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Nasıl derleme/güvenli kullanıcı veri örneği çalıştırma

* Gizli dizi Yöneticisi Aracı ile parola ayarlayın:

  `dotnet user-secrets set SeedUserPW <pw>`

* Veritabanını Güncelleştir:

  `dotnet ef database update`

* Projede HTTPS'yi etkinleştirme
