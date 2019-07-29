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
# <a name="hash-passwords-in-aspnet-core"></a>ASP.NET Core 'de karma parolalar

Veri koruma kodu tabanı, şifreleme anahtar türetme işlevlerini içeren *Microsoft. AspNetCore. Cryptography. Keytüretme* paketini içerir. Bu paket tek başına bir bileşendir ve veri koruma sisteminin geri kalanı üzerinde hiçbir bağımlılığı yoktur. Tamamen bağımsız olarak kullanılabilir. Kaynak, bir kolaylık olarak veri koruma kodu tabanının yanında bulunur.

Paket şu anda [PBKDF2 algoritmasını](https://tools.ietf.org/html/rfc2898#section-5.2)kullanarak `KeyDerivation.Pbkdf2` bir parolayı karma olarak sağlayan bir yöntem sunmaktadır. Bu API .NET Framework var olan [Rfc2898DeriveBytes türüne](/dotnet/api/system.security.cryptography.rfc2898derivebytes)çok benzer, ancak üç önemli ayrımlar vardır:

1. `HMACSHA512` `HMACSHA1` `HMACSHA1` `Rfc2898DeriveBytes` Yöntemi birden çok prfs (Şu anda, `HMACSHA256`ve) kullanmayı destekler, ancak tür yalnızca destekler. `KeyDerivation.Pbkdf2`

2. `KeyDerivation.Pbkdf2` Yöntemi, geçerli işletim sistemini algılar ve yordamın en iyi duruma getirilmiş uygulamasını seçme girişimlerini, belirli durumlarda çok daha iyi performans sağlar. (Windows 8 ' de, 10 ' un `Rfc2898DeriveBytes`aktarım hızını yaklaşık olarak sunar.)

3. `KeyDerivation.Pbkdf2` Yöntemi, çağıranın tüm parametreleri (anahtar, prf ve yineleme sayısı) belirtmesini gerektirir. `Rfc2898DeriveBytes` Türü bunlar için varsayılan değerleri sağlar.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Gerçek dünya kullanım örneği için ASP.NET Core kimliğin `PasswordHasher` türü için [kaynak koda](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) bakın.
