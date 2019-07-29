---
title: ASP.NET Core 'de karma parolalar
author: rick-anderson
description: ASP.NET Core veri koruma API 'Lerini kullanarak parolaların karmasını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: bd4b8fcf6a5a16a86ada97bbd3519f872d1417b7
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602459"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="87080-103">ASP.NET Core 'de karma parolalar</span><span class="sxs-lookup"><span data-stu-id="87080-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="87080-104">Veri koruma kodu tabanı, şifreleme anahtar türetme işlevlerini içeren *Microsoft. AspNetCore. Cryptography. Keytüretme* paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="87080-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="87080-105">Bu paket tek başına bir bileşendir ve veri koruma sisteminin geri kalanı üzerinde hiçbir bağımlılığı yoktur.</span><span class="sxs-lookup"><span data-stu-id="87080-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="87080-106">Tamamen bağımsız olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="87080-106">It can be used completely independently.</span></span> <span data-ttu-id="87080-107">Kaynak, bir kolaylık olarak veri koruma kodu tabanının yanında bulunur.</span><span class="sxs-lookup"><span data-stu-id="87080-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="87080-108">Paket şu anda [PBKDF2 algoritmasını](https://tools.ietf.org/html/rfc2898#section-5.2)kullanarak `KeyDerivation.Pbkdf2` bir parolayı karma olarak sağlayan bir yöntem sunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="87080-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="87080-109">Bu API .NET Framework var olan [Rfc2898DeriveBytes türüne](/dotnet/api/system.security.cryptography.rfc2898derivebytes)çok benzer, ancak üç önemli ayrımlar vardır:</span><span class="sxs-lookup"><span data-stu-id="87080-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="87080-110">`HMACSHA512` `HMACSHA1` `HMACSHA1` `Rfc2898DeriveBytes` Yöntemi birden çok prfs (Şu anda, `HMACSHA256`ve) kullanmayı destekler, ancak tür yalnızca destekler. `KeyDerivation.Pbkdf2`</span><span class="sxs-lookup"><span data-stu-id="87080-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="87080-111">`KeyDerivation.Pbkdf2` Yöntemi, geçerli işletim sistemini algılar ve yordamın en iyi duruma getirilmiş uygulamasını seçme girişimlerini, belirli durumlarda çok daha iyi performans sağlar.</span><span class="sxs-lookup"><span data-stu-id="87080-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="87080-112">(Windows 8 ' de, 10 ' un `Rfc2898DeriveBytes`aktarım hızını yaklaşık olarak sunar.)</span><span class="sxs-lookup"><span data-stu-id="87080-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="87080-113">`KeyDerivation.Pbkdf2` Yöntemi, çağıranın tüm parametreleri (anahtar, prf ve yineleme sayısı) belirtmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="87080-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="87080-114">`Rfc2898DeriveBytes` Türü bunlar için varsayılan değerleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="87080-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="87080-115">Gerçek dünya kullanım örneği için ASP.NET Core kimliğin `PasswordHasher` türü için [kaynak koda](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) bakın.</span><span class="sxs-lookup"><span data-stu-id="87080-115">See the [source code](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
