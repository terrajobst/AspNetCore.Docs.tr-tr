---
title: ASP.NET Core karma parolalar
author: rick-anderson
description: ASP.NET Core veri koruma API'lerini kullanarak parolaları karma öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 882ac9b256b0cdf5fd19dc4bd2757cac7e8ecad3
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538385"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="20858-103">ASP.NET Core karma parolalar</span><span class="sxs-lookup"><span data-stu-id="20858-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="20858-104">Veri koruma kod tabanı içeren bir paket *Microsoft.AspNetCore.Cryptography.KeyDerivation* şifreleme anahtar türetme işlevleri içerir.</span><span class="sxs-lookup"><span data-stu-id="20858-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="20858-105">Bu paket, bir tek başına bileşeni ve veri koruma sistemin geri kalanı üzerinde bağımlılık yok.</span><span class="sxs-lookup"><span data-stu-id="20858-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="20858-106">Tamamen bağımsız olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="20858-106">It can be used completely independently.</span></span> <span data-ttu-id="20858-107">Veri koruma kod kolaylık tabanı yanı sıra kaynağı yok.</span><span class="sxs-lookup"><span data-stu-id="20858-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="20858-108">Paket şu anda bir yöntem sunar `KeyDerivation.Pbkdf2` sağlayan kullanarak bir parolayı karma [PBKDF2 algoritması](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="20858-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="20858-109">Bu API .NET Framework'ün varolan çok benzer [Rfc2898DeriveBytes türü](/dotnet/api/system.security.cryptography.rfc2898derivebytes), ancak üç önemli farklılıklar vardır:</span><span class="sxs-lookup"><span data-stu-id="20858-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="20858-110">`KeyDerivation.Pbkdf2` Yöntem, birden çok PRFs tüketen destekler (şu anda `HMACSHA1`, `HMACSHA256`, ve `HMACSHA512`) bilgileriyse `Rfc2898DeriveBytes` destekler yazın `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="20858-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="20858-111">`KeyDerivation.Pbkdf2` Yöntemi geçerli işletim sistemini algılar ve bazı durumlarda çok daha iyi performans sağlayan yordamının en iyileştirilmiş uygulamayı seçme dener.</span><span class="sxs-lookup"><span data-stu-id="20858-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="20858-112">(İsteğe bağlı olarak Windows 8'de yaklaşık 10 aktarım hızı x sunar `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="20858-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="20858-113">`KeyDerivation.Pbkdf2` Yöntemi çağıran tüm parametreleri belirtmek gerektirir (salt, PRF ve yineleme sayısı).</span><span class="sxs-lookup"><span data-stu-id="20858-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="20858-114">`Rfc2898DeriveBytes` Türü için varsayılan değerleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="20858-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="20858-115">Bkz. [kaynak kodu] (https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) için ASP.NET Core kimliğin `PasswordHasher` gerçek dünya için tür kullanım örneği.</span><span class="sxs-lookup"><span data-stu-id="20858-115">See the [source code]（https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
