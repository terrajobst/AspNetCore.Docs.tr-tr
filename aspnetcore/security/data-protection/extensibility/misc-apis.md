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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="3b9e8-103">Çeşitli ASP.NET Core veri koruma API'ları</span><span class="sxs-lookup"><span data-stu-id="3b9e8-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="3b9e8-104">Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.</span><span class="sxs-lookup"><span data-stu-id="3b9e8-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="3b9e8-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="3b9e8-105">ISecret</span></span>

<span data-ttu-id="3b9e8-106">`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3b9e8-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="3b9e8-107">Aşağıdaki API yüzeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="3b9e8-107">It contains the following API surface:</span></span>

* <span data-ttu-id="3b9e8-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="3b9e8-108">`Length`: `int`</span></span>

* <span data-ttu-id="3b9e8-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="3b9e8-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="3b9e8-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="3b9e8-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="3b9e8-111">`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur.</span><span class="sxs-lookup"><span data-stu-id="3b9e8-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="3b9e8-112">Bu API geçen bir parametre olarak arabellek neden döndürme yerine bir `byte[]` doğrudan bu çağıran yönetilen çöp toplayıcı gizli maruz sınırlama arabellek nesnesi PIN fırsatı veren özelliktir.</span><span class="sxs-lookup"><span data-stu-id="3b9e8-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="3b9e8-113">`Secret` Türü olan somut bir uyarlamasını `ISecret` gizli değer işlemdeki bellekte depolandığı.</span><span class="sxs-lookup"><span data-stu-id="3b9e8-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="3b9e8-114">Windows platformlarında aracılığıyla gizli değer şifrelenir [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="3b9e8-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
