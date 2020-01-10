---
title: ASP.NET Core 'de karma parolalar
author: rick-anderson
description: ASP.NET Core veri koruma API 'Lerini kullanarak parolaların karmasını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 52532f9f4d7f9037609c8273deb8b6964d81c714
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828574"
---
# <a name="hash-passwords-in-aspnet-core"></a>ASP.NET Core 'de karma parolalar

Veri koruma kodu tabanı, şifreleme anahtar türetme işlevlerini içeren *Microsoft. AspNetCore. Cryptography. Keytüretme* paketini içerir. Bu paket tek başına bir bileşendir ve veri koruma sisteminin geri kalanı üzerinde hiçbir bağımlılığı yoktur. Tamamen bağımsız olarak kullanılabilir. Kaynak, bir kolaylık olarak veri koruma kodu tabanının yanında bulunur.

Paket şu anda [PBKDF2 algoritmasını](https://tools.ietf.org/html/rfc2898#section-5.2)kullanarak bir parolayı karma olarak sağlayan bir yöntem `KeyDerivation.Pbkdf2` sunmaktadır. Bu API .NET Framework var olan [Rfc2898DeriveBytes türüne](/dotnet/api/system.security.cryptography.rfc2898derivebytes)çok benzer, ancak üç önemli ayrımlar vardır:

1. `KeyDerivation.Pbkdf2` yöntemi birden çok PRFs (Şu anda `HMACSHA1`, `HMACSHA256`ve `HMACSHA512`) kullanmayı destekler, ancak `Rfc2898DeriveBytes` türü yalnızca `HMACSHA1`destekler.

2. `KeyDerivation.Pbkdf2` yöntemi geçerli işletim sistemini algılar ve yordamın en iyi duruma getirilmiş uygulamasını seçerek belirli durumlarda daha iyi performans sağlar. (Windows 8 ' de, `Rfc2898DeriveBytes`üretilen iş hızına 10 kat etrafında sahiptir.)

3. `KeyDerivation.Pbkdf2` yöntemi, çağıranın tüm parametreleri (anahtar, PRF ve yineleme sayısı) belirtmesini gerektirir. `Rfc2898DeriveBytes` türü bunlar için varsayılan değerleri sağlar.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Gerçek dünya kullanım örneği için ASP.NET Core kimliğin `PasswordHasher` türü için [kaynak koda](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs) bakın.
