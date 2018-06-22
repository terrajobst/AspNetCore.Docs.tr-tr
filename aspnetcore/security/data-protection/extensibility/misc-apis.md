---
title: Çeşitli ASP.NET Core veri koruma API'ları
author: rick-anderson
description: ASP.NET Core veri koruma ISecret arabirimi hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279161"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Çeşitli ASP.NET Core veri koruma API'ları

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.

## <a name="isecret"></a>ISecret

`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder. Aşağıdaki API yüzeyi içerir:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur. Bu API geçen bir parametre olarak arabellek neden döndürme yerine bir `byte[]` doğrudan bu çağıran yönetilen çöp toplayıcı gizli maruz sınırlama arabellek nesnesi PIN fırsatı veren özelliktir.

`Secret` Türü olan somut bir uyarlamasını `ISecret` gizli değer işlemdeki bellekte depolandığı. Windows platformlarında aracılığıyla gizli değer şifrelenir [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
