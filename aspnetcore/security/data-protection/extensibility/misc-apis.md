---
title: Çeşitli ASP.NET Core veri koruma API'ları
author: rick-anderson
description: ASP.NET Core veri koruma ISecret arabirimi hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072770"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="ac45b-103">Çeşitli ASP.NET Core veri koruma API'ları</span><span class="sxs-lookup"><span data-stu-id="ac45b-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="ac45b-104">Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.</span><span class="sxs-lookup"><span data-stu-id="ac45b-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="ac45b-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="ac45b-105">ISecret</span></span>

<span data-ttu-id="ac45b-106">`ISecret` Arabirimi şifreleme anahtar malzemesi gibi gizli bir değeri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="ac45b-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="ac45b-107">Aşağıdaki API yüzeyi içerir:</span><span class="sxs-lookup"><span data-stu-id="ac45b-107">It contains the following API surface:</span></span>

* <span data-ttu-id="ac45b-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="ac45b-108">`Length`: `int`</span></span>

* <span data-ttu-id="ac45b-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="ac45b-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="ac45b-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="ac45b-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="ac45b-111">`WriteSecretIntoBuffer` Yöntemi sağlanan arabellek ham gizli değer ile doldurur.</span><span class="sxs-lookup"><span data-stu-id="ac45b-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="ac45b-112">Bu API geçen bir parametre olarak arabellek neden döndürme yerine bir `byte[]` doğrudan bu çağıran yönetilen çöp toplayıcı gizli maruz sınırlama arabellek nesnesi PIN fırsatı veren özelliktir.</span><span class="sxs-lookup"><span data-stu-id="ac45b-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="ac45b-113">`Secret` Türü olan somut bir uyarlamasını `ISecret` gizli değer işlemdeki bellekte depolandığı.</span><span class="sxs-lookup"><span data-stu-id="ac45b-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="ac45b-114">Windows platformlarında aracılığıyla gizli değer şifrelenir [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="ac45b-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
