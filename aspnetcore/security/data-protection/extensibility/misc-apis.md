---
title: Çeşitli ASP.NET Core veri koruma API 'Leri
author: rick-anderson
description: ASP.NET Core Data Protection ısecret arabirimi hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666084"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Çeşitli ASP.NET Core veri koruma API 'Leri

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.

## <a name="isecret"></a>Isecret

`ISecret` arabirimi, şifreleme anahtar malzemesi gibi bir gizli değeri temsil eder. Aşağıdaki API yüzeyini içerir:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` yöntemi, sağlanan arabelleği ham gizli değer değeriyle doldurur. Bu API 'nin arabelleği bir `byte[]` döndürmek yerine bir parametre olarak aldığı neden, bu, çağrıyı yapana, yönetilen çöp toplayıcısıyla gizli dizi ile sınırlama sağlayan bir arabellek nesnesini sabitleme fırsatı verir.

`Secret` türü, gizli değerin işlem içi bellekte depolandığı `ISecret` somut bir uygulamasıdır. Windows platformlarında gizli değeri [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx)aracılığıyla şifrelenir.
