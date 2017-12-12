---
title: "Çeşitli API'ler"
author: rick-anderson
description: "Bu belge, ASP.NET Core veri koruma ISecret arabirimi özetlenmektedir."
keywords: ASP.NET Core, veri koruma, ISecret
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="ae1b7-104">Çeşitli API'ler</span><span class="sxs-lookup"><span data-stu-id="ae1b7-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ae1b7-105">Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.</span><span class="sxs-lookup"><span data-stu-id="ae1b7-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ae1b7-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="ae1b7-106">ISecret</span></span>

<span data-ttu-id="ae1b7-107">`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ae1b7-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ae1b7-108">Aşağıdaki API yüzeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="ae1b7-108">It contains the following API surface:</span></span>

* <span data-ttu-id="ae1b7-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ae1b7-109">`Length`: `int`</span></span>

* <span data-ttu-id="ae1b7-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ae1b7-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ae1b7-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ae1b7-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ae1b7-112">`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur.</span><span class="sxs-lookup"><span data-stu-id="ae1b7-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ae1b7-113">Bu API geçen bir parametre olarak arabellek neden döndürme yerine bir `byte[]` doğrudan bu çağıran yönetilen çöp toplayıcı gizli maruz sınırlama arabellek nesnesi PIN fırsatı veren özelliktir.</span><span class="sxs-lookup"><span data-stu-id="ae1b7-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ae1b7-114">`Secret` Türü olan somut bir uyarlamasını `ISecret` gizli değer işlemdeki bellekte depolandığı.</span><span class="sxs-lookup"><span data-stu-id="ae1b7-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ae1b7-115">Windows platformlarında aracılığıyla gizli değer şifrelenir [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ae1b7-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
