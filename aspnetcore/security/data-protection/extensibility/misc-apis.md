---
title: "Çeşitli API'ler"
author: rick-anderson
description: "Bu belge, ASP.NET Core veri koruma ISecret arabirimi özetlenmektedir."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="6f5f8-103">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="6f5f8-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="6f5f8-104">Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.</span><span class="sxs-lookup"><span data-stu-id="6f5f8-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="6f5f8-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="6f5f8-105">ISecret</span></span>

<span data-ttu-id="6f5f8-106">`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6f5f8-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="6f5f8-107">Aşağıdaki API yüzeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="6f5f8-107">It contains the following API surface:</span></span>

* <span data-ttu-id="6f5f8-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="6f5f8-108">`Length`: `int`</span></span>

* <span data-ttu-id="6f5f8-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="6f5f8-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="6f5f8-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="6f5f8-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="6f5f8-111">`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur.</span><span class="sxs-lookup"><span data-stu-id="6f5f8-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="6f5f8-112">Bu API geçen bir parametre olarak arabellek neden döndürme yerine bir `byte[]` doğrudan bu çağıran yönetilen çöp toplayıcı gizli maruz sınırlama arabellek nesnesi PIN fırsatı veren özelliktir.</span><span class="sxs-lookup"><span data-stu-id="6f5f8-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="6f5f8-113">`Secret` Türü olan somut bir uyarlamasını `ISecret` gizli değer işlemdeki bellekte depolandığı.</span><span class="sxs-lookup"><span data-stu-id="6f5f8-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="6f5f8-114">Windows platformlarında aracılığıyla gizli değer şifrelenir [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="6f5f8-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
