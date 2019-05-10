---
title: ASP.NET Core kimliği doğrulanmış şifreleme ayrıntıları
author: rick-anderson
description: ASP.NET Core veri koruma kimliği doğrulanmış şifreleme uygulama ayrıntıları öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902663"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>ASP.NET Core kimliği doğrulanmış şifreleme ayrıntıları

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Kimliği doğrulanmış şifreleme IDataProtector.Protect çağrıları işlemlerdir. Bu Idataprotector örnekte kendi kök IDataProtectionProvider türetmek için kullanılan amaçlı zinciri bağlıdır ve koruma yöntemi hem gizliliği ve kimlik doğrulama sunar.

IDataProtector.Protect bayt [] düz metin parametre alır ve aşağıda açıklanan biçimde bir bayt [] korumalı yükü üretir. (Var. Ayrıca bir dize düz metin parametresi alan ve bir dize korumalı yükü döndüren bir genişletme yöntemi aşırı yüklemesi Bu API kullanılıyorsa, korumalı yükü biçimde hala olan yapısı, aşağıda ancak olur [base64url kodlu](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Korumalı yük biçimi

Korumalı yük biçimi üç birincil bileşenden oluşur:

* Veri koruma sisteminde sürümünü tanımlayan bir 32-bit Sihirli üstbilgisi.

* Bu belirli yük korumak için kullanılan anahtarı tanımlar 128 bit anahtar kimliği.

* Korumalı yük kalan [bu anahtara göre kapsüllenmiş Şifreleyici özgü](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Aşağıdaki örnekte, bir AES 256 CBC + HMACSHA256 Şifreleyici tuşunu temsil eder ve yükü daha ayrıntılı şekilde ayrılır:
  * 128 bit anahtar değiştiricisi.
  * Bir 128-bit başlatma vektörü.
  * AES 256 CBC çıkış 48 bayt.
  * HMACSHA256 kimlik doğrulaması etiketi.

Bir korumalı örnek yük aşağıda gösterilmiştir.

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

(09 F0 C9 F0) sürümünü tanımlayan Sihirli üstbilgi ilk 32 bit veya 4 bayt yukarıda yük biçimi arasındadır

Sonraki 128 bit ya da 16 bayt olan anahtar tanımlayıcısı (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Kalan yükü içerir ve kullanılan biçimi özgüdür.

> [!WARNING]
> Belirli bir anahtarın için korunan tüm yükleri aynı 20 baytlık (Sihirli değeri, anahtar kimliği) üst bilgisi ile başlar. Yöneticiler, bir yük oluşturulduğunda yaklaşık olarak belirlemenizi sağlayan bu olgu tanılama amacıyla kullanabilir. Örneğin, yukarıdaki yükü {0c819c80-6619-4019-9536-53f8aaffee57} anahtarına karşılık gelir. Anahtar deposu denetledikten sonra bu belirli bir anahtarın etkinleştirme tarihine gelindiğinden 2015-01-01 ve 2015-03-01, sona erme tarihine gelindiğinden sonra varsayımında şüphelenilebilir fark ederseniz yük (değiştirilmiş değil) sağlar, pencere içinde oluşturulan veya küçük olması Her iki tarafında karamelli faktörü.
