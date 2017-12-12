---
title: Parola karma
author: rick-anderson
description: "Bu belge, ASP.NET Core veri koruma API'ları kullanarak parola karma açıklanmaktadır."
keywords: "ASP.NET Core, veri koruma, parola karması"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 982a1eb2-1e6f-4909-896f-82784364472d
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: da9f505f58f18f7ab3b93753bae079eb976b3939
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="password-hashing"></a>Parola karma

Bir paketi temel veri koruma kodu içerir *Microsoft.AspNetCore.Cryptography.KeyDerivation* şifreleme anahtar türetme işlevler içerir. Bu paket, bir tek başına bileşenidir ve veri koruma sisteminde rest üzerinde hiçbir bağımlılığı yoktur. Tamamen bağımsız olarak kullanılabilir. Veri koruma kod kolaylık tabanını yanında kaynağı yok.

Paket şu anda bir yöntem sunar `KeyDerivation.Pbkdf2` sağlayan kullanarak bir parolayı karma [PBKDF2 algoritması](https://tools.ietf.org/html/rfc2898#section-5.2). Bu API .NET Framework'ün varolan çok benzer [Rfc2898DeriveBytes türü](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), ancak üç önemli farklılıklar vardır:

1. `KeyDerivation.Pbkdf2` Yöntemini destekleyen birden çok PRFs kullanma (şu anda `HMACSHA1`, `HMACSHA256`, ve `HMACSHA512`), ancak `Rfc2898DeriveBytes` destekler yazın `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Yöntemi geçerli işletim sistemini algılar ve bazı durumlarda çok daha iyi performans sağlayan yordamının en iyileştirilmiş uygulama seçmek çalışır. (İsteğe bağlı olarak Windows 8'de yaklaşık 10 üretimini x sunar `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Gerektirdiğine çağıran tüm parametreleri belirtin (salt, PRF ve yineleme sayısı). `Rfc2898DeriveBytes` Türü için varsayılan değerleri sağlar.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

ASP.NET Core kimliğin için kaynak koduna bakın `PasswordHasher` türü gerçek dünya için kullanım örneği.
