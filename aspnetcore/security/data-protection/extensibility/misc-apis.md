---
title: ASP.NET Core çeşitli veri koruma API'lerini
author: rick-anderson
description: ASP.NET Core veri koruma ISecret arabirimi hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902660"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>ASP.NET Core çeşitli veri koruma API'lerini

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.

## <a name="isecret"></a>ISecret

`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder. Aşağıdaki API yüzeyi aşağıdakileri içerir:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur. Bu API arabellek bir parametre olarak alan nedeni döndürmek yerine bir `byte[]` doğrudan bu çağıran atık toplayıcı yönetilen gizli maruz kalma riskinizi sınırlama arabellek nesne sabitlemek için Fırsat vermesidir.

`Secret` Türüdür, somut bir uygulama `ISecret` işlem içi bellekte gizli değer depolandığı. Windows platformlarında, gizli değer aracılığıyla şifrelenmiş [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
