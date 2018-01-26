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
ms.openlocfilehash: 2a59a7531c946927b4b266d5dfa9af46afdae411
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="authenticated-encryption-details"></a><span data-ttu-id="b1f7d-103">Kimliği doğrulanmış şifreleme ayrıntıları</span><span class="sxs-lookup"><span data-stu-id="b1f7d-103">Authenticated encryption details</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="b1f7d-104">Kimliği doğrulanmış şifreleme işlemlerini IDataProtector.Protect çağrıları aynıdır.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="b1f7d-105">Gizlilik ve kimlik doğrulama koruma yöntemi sunar ve bu belirli Idataprotector örneğinin kendi kök IDataProtectionProvider türetmek için kullanılan amacı zinciri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="b1f7d-106">IDataProtector.Protect byte [] düz metin parametresini alır ve biçimini aşağıda açıklanan byte [] korumalı yük de üretir.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="b1f7d-107">(Ayrıca vardır bir dize düz metin parametresi alan ve dize korumalı yükü döndüren bir genişletme yöntemi aşırı.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="b1f7d-108">Bu API kullanılırsa, korumalı yük biçimi devam ediyor yapısı, aşağıda ancak yüklenir [base64url kodlanmış](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="b1f7d-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="b1f7d-109">Korumalı yük biçimi</span><span class="sxs-lookup"><span data-stu-id="b1f7d-109">Protected payload format</span></span>

<span data-ttu-id="b1f7d-110">Korumalı yük biçimi üç birincil bileşenden oluşur:</span><span class="sxs-lookup"><span data-stu-id="b1f7d-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="b1f7d-111">Veri koruma sisteminde sürümünü belirleyen bir 32 bit Sihirli üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="b1f7d-112">Bu belirli yükü korumak için kullanılan anahtarı tanımlar 128-bit anahtar kimliği.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="b1f7d-113">Korumalı yük kalan [bu anahtarı tarafından kapsüllenmiş Şifreleyici belirli](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="b1f7d-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="b1f7d-114">Aşağıdaki örnekte anahtarı temsil eden bir AES 256 CBC + HMACSHA256 Şifreleyici ve yükü daha ayrıntılı şekilde ayrılır: \* A 128-bit anahtar değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: \* A 128-bit key modifier.</span></span> <span data-ttu-id="b1f7d-115">\* Bir 128-bit başlatma vektörü.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-115">\* A 128-bit initialization vector.</span></span> <span data-ttu-id="b1f7d-116">\* AES 256 CBC çıktı 48 bayt sayısı.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-116">\* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="b1f7d-117">\* Bir HMACSHA256 kimlik doğrulaması etiketi.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-117">\* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="b1f7d-118">Bir örnek korumalı yükü aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-118">A sample protected payload is illustrated below.</span></span>

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

<span data-ttu-id="b1f7d-119">(09 F0 C9 F0) sürümünü tanımlayan Sihirli üstbilgi ilk 32 bit ya da 4 bayt üstünde yük biçimi arasındadır</span><span class="sxs-lookup"><span data-stu-id="b1f7d-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="b1f7d-120">Sonraki 128 bit veya 16 bayt tanımlayıcısıdır anahtar (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="b1f7d-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="b1f7d-121">Kalan yükünü içeren ve kullanılan biçimi özeldir.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="b1f7d-122">Belirli bir anahtarı korunan tüm yüklerini aynı 20 baytlık (Sihirli değeri, anahtar kimliği) üstbilgisi ile başlar.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="b1f7d-123">Yöneticiler bu olgu tanılama amacıyla bir yükü oluşturulduğunda yaklaşık için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="b1f7d-124">Örneğin, yukarıdaki yükü {0c819c80-6619-4019-9536-53f8aaffee57} anahtarına karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="b1f7d-125">Anahtar deposu denetledikten sonra bu özel anahtarın etkinleştirme tarihi 2015-01-01 idi ve sona erme tarihini 2015-03-01 edildi sonra varsayımında makul görürseniz Yükü (değiştirilmiş değil) verin, penceresi içinde oluşturulan veya küçük bir alın Her iki tarafında fudge faktörü.</span><span class="sxs-lookup"><span data-stu-id="b1f7d-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
