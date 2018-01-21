---
title: "Kimliği doğrulanmış şifreleme ayrıntıları"
author: rick-anderson
description: "Bu belge anahatları ASP.NET Core veri koruma uygulama ayrıntılarını şifreleme kimlik doğrulaması."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 23d45c38b28c84b1c1c9b92a0ec299f0bed1d4e4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="authenticated-encryption-details"></a>Kimliği doğrulanmış şifreleme ayrıntıları

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Kimliği doğrulanmış şifreleme işlemlerini IDataProtector.Protect çağrıları aynıdır. Gizlilik ve kimlik doğrulama koruma yöntemi sunar ve bu belirli Idataprotector örneğinin kendi kök IDataProtectionProvider türetmek için kullanılan amacı zinciri bağlıdır.

IDataProtector.Protect byte [] düz metin parametresini alır ve biçimini aşağıda açıklanan byte [] korumalı yük de üretir. (Ayrıca vardır bir dize düz metin parametresi alan ve dize korumalı yükü döndüren bir genişletme yöntemi aşırı. Bu API kullanılırsa, korumalı yük biçimi devam ediyor yapısı, aşağıda ancak yüklenir [base64url kodlanmış](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Korumalı yük biçimi

Korumalı yük biçimi üç birincil bileşenden oluşur:

* Veri koruma sisteminde sürümünü belirleyen bir 32 bit Sihirli üstbilgisi.

* Bu belirli yükü korumak için kullanılan anahtarı tanımlar 128-bit anahtar kimliği.

* Korumalı yük kalan [bu anahtarı tarafından kapsüllenmiş Şifreleyici belirli](subkeyderivation.md#data-protection-implementation-subkey-derivation). Aşağıdaki örnekte anahtarı temsil eden bir AES 256 CBC + HMACSHA256 Şifreleyici ve yükü daha ayrıntılı şekilde ayrılır: * A 128-bit anahtar değiştiricisi. * Bir 128-bit başlatma vektörü. * AES 256 CBC çıktı 48 bayt sayısı. * Bir HMACSHA256 kimlik doğrulaması etiketi.

Bir örnek korumalı yükü aşağıda gösterilmiştir.

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

(09 F0 C9 F0) sürümünü tanımlayan Sihirli üstbilgi ilk 32 bit ya da 4 bayt üstünde yük biçimi arasındadır

Sonraki 128 bit veya 16 bayt tanımlayıcısıdır anahtar (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Kalan yükünü içeren ve kullanılan biçimi özeldir.

>[!WARNING]
> Belirli bir anahtarı korunan tüm yüklerini aynı 20 baytlık (Sihirli değeri, anahtar kimliği) üstbilgisi ile başlar. Yöneticiler bu olgu tanılama amacıyla bir yükü oluşturulduğunda yaklaşık için kullanabilirsiniz. Örneğin, yukarıdaki yükü {0c819c80-6619-4019-9536-53f8aaffee57} anahtarına karşılık gelir. Anahtar deposu denetledikten sonra bu özel anahtarın etkinleştirme tarihi 2015-01-01 idi ve sona erme tarihini 2015-03-01 edildi sonra varsayımında makul görürseniz Yükü (değiştirilmiş değil) verin, penceresi içinde oluşturulan veya küçük bir alın Her iki tarafında fudge faktörü.
