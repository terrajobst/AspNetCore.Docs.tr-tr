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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="4798e-103">ASP.NET Core çeşitli veri koruma API'lerini</span><span class="sxs-lookup"><span data-stu-id="4798e-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="4798e-104">Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.</span><span class="sxs-lookup"><span data-stu-id="4798e-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="4798e-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="4798e-105">ISecret</span></span>

<span data-ttu-id="4798e-106">`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4798e-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="4798e-107">Aşağıdaki API yüzeyi aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="4798e-107">It contains the following API surface:</span></span>

* <span data-ttu-id="4798e-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="4798e-108">`Length`: `int`</span></span>

* <span data-ttu-id="4798e-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="4798e-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="4798e-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="4798e-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="4798e-111">`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur.</span><span class="sxs-lookup"><span data-stu-id="4798e-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="4798e-112">Bu API arabellek bir parametre olarak alan nedeni döndürmek yerine bir `byte[]` doğrudan bu çağıran atık toplayıcı yönetilen gizli maruz kalma riskinizi sınırlama arabellek nesne sabitlemek için Fırsat vermesidir.</span><span class="sxs-lookup"><span data-stu-id="4798e-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="4798e-113">`Secret` Türüdür, somut bir uygulama `ISecret` işlem içi bellekte gizli değer depolandığı.</span><span class="sxs-lookup"><span data-stu-id="4798e-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="4798e-114">Windows platformlarında, gizli değer aracılığıyla şifrelenmiş [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="4798e-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
