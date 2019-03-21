---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210112"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="3a323-101">Nasıl derleme/güvenli kullanıcı veri örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="3a323-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="3a323-102">Gizli dizi Yöneticisi Aracı ile parola ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3a323-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="3a323-103">Veritabanını Güncelleştir:</span><span class="sxs-lookup"><span data-stu-id="3a323-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="3a323-104">Projede HTTPS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="3a323-104">Enable HTTPS in the project</span></span>
