---
title: "İçerik üstbilgileri"
author: rick-anderson
description: "Bu belge ASP.NET Core veri koruma içerik üstbilgileri uygulama ayrıntılarını özetlemektedir."
keywords: "ASP.NET Core, veri koruma, içerik üstbilgileri"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d026a58c-67f4-411e-a410-c35f29c2c517
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: eb8e4c9ad67d3046648aea1b45f4a675b41b3ec0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="context-headers"></a><span data-ttu-id="02621-104">İçerik üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="02621-104">Context headers</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="02621-105">Arka plan ve teorik</span><span class="sxs-lookup"><span data-stu-id="02621-105">Background and theory</span></span>

<span data-ttu-id="02621-106">Veri koruma sisteminde şifreleme hizmetleri sağlayan bir nesne kimliği doğrulanmış bir "anahtar" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="02621-106">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="02621-107">Her anahtar benzersiz bir kimlik (GUID) tarafından tanımlanır ve onunla taşıyan algoritmik bilgi ve entropic malzeme.</span><span class="sxs-lookup"><span data-stu-id="02621-107">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="02621-108">Her anahtar taşımak benzersiz entropi, ancak sistem, zorunlu kılamaz ve ayrıca varolan anahtarı anahtar halkası algoritmik bilgilerinin değiştirerek anahtar halkası el ile değişebilir geliştiriciler için hesap ihtiyacımız yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="02621-108">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="02621-109">Bu gibi durumlarda verilen bizim güvenlik gereksinimlerini elde etmek için veri koruma sisteminde bir kavramı vardır [şifreleme çevikliği](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), güvenli bir şekilde tek bir entropic değer arasında birden çok şifreleme algoritmaları kullanarak izin verir.</span><span class="sxs-lookup"><span data-stu-id="02621-109">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="02621-110">Şifreleme çevikliği destekleyen çoğu sistemi yükünün algoritması hakkında bazı tanımlama bilgileri ekleyerek bunu.</span><span class="sxs-lookup"><span data-stu-id="02621-110">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="02621-111">Algoritma 's OID genellikle iyi bir aday içindir.</span><span class="sxs-lookup"><span data-stu-id="02621-111">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="02621-112">Ancak, aynı algoritma belirtmek için birden çok yolu vardır içine karşılaştık bir sorun olduğunu: "AES" (CNG) ve yönetilen Aes, AesManaged, AesCryptoServiceProvider, AesCng ve (belirli parametreleri verilir) RijndaelManaged sınıflarıdır tüm aslında aynı tüm bunların doğru OID için bir eşleme korumak şey ve biz gerekir.</span><span class="sxs-lookup"><span data-stu-id="02621-112">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="02621-113">Bir geliştirici özel bir algoritma (veya hatta başka bir AES uyarlamasını!) sağlamak istiyorsanız, OID bize iletmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="02621-113">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="02621-114">Bu ek kayıt adım sistem yapılandırması özellikle Acı verici sağlar.</span><span class="sxs-lookup"><span data-stu-id="02621-114">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="02621-115">Geri atlama, biz sorun yanlış yönünden yaklaşmakta olduğunu karar.</span><span class="sxs-lookup"><span data-stu-id="02621-115">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="02621-116">OID, algoritma nedir bildirir, ancak biz aslında bu hakkında önemli değil.</span><span class="sxs-lookup"><span data-stu-id="02621-116">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="02621-117">Biz tek bir entropic değer güvenli bir şekilde iki farklı algoritmalara kullanmanız gerekiyorsa, bize algoritmaları gerçekte ne olduğunu öğrenmek gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="02621-117">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="02621-118">Ne biz gerçekte önem verdiğiniz nasıl davranırlar olur.</span><span class="sxs-lookup"><span data-stu-id="02621-118">What we actually care about is how they behave.</span></span> <span data-ttu-id="02621-119">Tüm makul simetrik blok şifre de güçlü geçici rastgele sıralamaya (PRP) algoritmasıdır: (modu, IV, düz metin zincirleme anahtarı) girişleri düzeltin ve ciphertext çıktı olasılık yayma ile diğer simetrik blok şifre farklı olacaktır aynı giriş verilen algoritması.</span><span class="sxs-lookup"><span data-stu-id="02621-119">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="02621-120">Benzer şekilde, tüm makul anahtarlı karma işlevi de güçlü bir geçici rastgele işlevi (PRF) olduğunu ve sabit bir giriş kümesi çıktısını son derece diğer anahtarlı karma işlevinden farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="02621-120">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="02621-121">Bir bağlam başlığını oluşturmak için bu kavramı güçlü PRPs ve PRFs kullanırız.</span><span class="sxs-lookup"><span data-stu-id="02621-121">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="02621-122">Bu içerik üstbilgisi kararlı bir parmak izi temelde herhangi belirtilen işlem için kullanılan algoritmalar üzerinden çalışır ve veri koruma sistemi tarafından gereken şifreleme çevikliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="02621-122">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="02621-123">Bu üst yeniden üretilebilir ve daha sonra bir parçası olarak kullanılan [alt anahtar türetme işlem](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="02621-123">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="02621-124">İçerik Üstbilgisi işlemi temel algoritmalarının modlarını bağlı olarak oluşturmak için iki farklı yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="02621-124">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="02621-125">CBC modunda şifreleme + HMAC kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="02621-125">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="02621-126">İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="02621-126">The context header consists of the following components:</span></span>

* <span data-ttu-id="02621-127">[16 bit] 00 değeri bir işaretçisi 00 "CBC şifreleme + HMAC kimlik doğrulaması" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="02621-127">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="02621-128">[32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) simetrik blok şifreleme algoritması.</span><span class="sxs-lookup"><span data-stu-id="02621-128">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="02621-129">[32 bit] Simetrik blok şifreleme algoritmasının blok boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="02621-129">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="02621-130">[32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) HMAC algoritması.</span><span class="sxs-lookup"><span data-stu-id="02621-130">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="02621-131">(Şu anda anahtar boyutu her zaman Özet boyutu eşleşir.)</span><span class="sxs-lookup"><span data-stu-id="02621-131">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="02621-132">[32 bit] Özet boyutunu (bayt cinsinden, big endian) HMAC algoritması.</span><span class="sxs-lookup"><span data-stu-id="02621-132">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="02621-133">EncCBC (K_E, IV, ""), boş bir dize giriş verilen simetrik blok şifreleme algoritması çıktısını olduğu ve IV tüm sıfır vektör olduğu.</span><span class="sxs-lookup"><span data-stu-id="02621-133">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="02621-134">K_E yapımı aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="02621-134">The construction of K_E is described below.</span></span>

* <span data-ttu-id="02621-135">MAC (K_H, ""), boş bir dize giriş verilen HMAC algoritması çıktısını olduğu.</span><span class="sxs-lookup"><span data-stu-id="02621-135">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="02621-136">K_H yapımı aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="02621-136">The construction of K_H is described below.</span></span>

<span data-ttu-id="02621-137">İdeal olarak, biz K_E ve K_H için tüm sıfır vektörlerinin geçirebilirdiniz.</span><span class="sxs-lookup"><span data-stu-id="02621-137">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="02621-138">Ancak, burada temel algoritma, tüm sıfır vektör gibi basit veya repeatable bir düzeni kullanarak önleyen (özellikle DES ve 3DES), herhangi bir işlem gerçekleştirmeden önce zayıf anahtarların bulunmadığını kontrol eder durumdan kaçınmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="02621-138">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="02621-139">Bunun yerine, biz NIST SP800 108 KDF sayaç modunda kullanın (bkz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sn 5.1) bir sıfır uzunluklu anahtar, etiket ve bağlamı ve temel PRF olarak HMACSHA512.</span><span class="sxs-lookup"><span data-stu-id="02621-139">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="02621-140">Biz türetilen | K_E | + | K_H | Çıkış, bayt sonra parçalayın sonucu K_E ve K_H kendilerini.</span><span class="sxs-lookup"><span data-stu-id="02621-140">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="02621-141">Matematiksel, bunu şu şekilde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="02621-141">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="02621-142">(K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = "")</span><span class="sxs-lookup"><span data-stu-id="02621-142">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="02621-143">Örnek: AES 192 CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="02621-143">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="02621-144">Örnek olarak, burada simetrik blok şifreleme algoritması AES 192 CBC ve doğrulama algoritması HMACSHA256 durumu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="02621-144">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="02621-145">Sistem aşağıdaki adımları kullanarak içerik üstbilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="02621-145">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="02621-146">İlk olarak, sağlar (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 192 bitlik ve | K_H | = belirtilen algoritmaları 256 bit.</span><span class="sxs-lookup"><span data-stu-id="02621-146">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="02621-147">Bu K_E için müşteri adayları 5BB6 =... 21DD ve K_H A04A =... Aşağıdaki örnekte 00A9:</span><span class="sxs-lookup"><span data-stu-id="02621-147">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="02621-148">Ardından, Enc_CBC işlem (K_E, IV, "") AES-192-IV verilen CBC için = 0 * ve yukarıdaki K_E.</span><span class="sxs-lookup"><span data-stu-id="02621-148">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="02621-149">Sonuç: F474B1872B3B53E4721DE19C0841DB6F =</span><span class="sxs-lookup"><span data-stu-id="02621-149">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="02621-150">Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA256 için.</span><span class="sxs-lookup"><span data-stu-id="02621-150">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="02621-151">Sonuç: D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C =</span><span class="sxs-lookup"><span data-stu-id="02621-151">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="02621-152">Tüm içerik üst bilgisi üretir:</span><span class="sxs-lookup"><span data-stu-id="02621-152">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="02621-153">Parmak izi kimliği doğrulanmış şifreleme algoritması çiftinin (AES 192 CBC şifreleme + HMACSHA256 doğrulama) bu içeriği başlığıdır.</span><span class="sxs-lookup"><span data-stu-id="02621-153">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="02621-154">Açıklandığı gibi bileşenleri [yukarıda](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) şunlardır:</span><span class="sxs-lookup"><span data-stu-id="02621-154">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="02621-155">işaretin (00 00)</span><span class="sxs-lookup"><span data-stu-id="02621-155">the marker (00 00)</span></span>

* <span data-ttu-id="02621-156">Blok şifre anahtar uzunluğu (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="02621-156">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="02621-157">Blok Şifre blok boyutu (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="02621-157">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="02621-158">HMAC anahtar uzunluğu (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="02621-158">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="02621-159">HMAC Özet boyutu (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="02621-159">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="02621-160">Blok şifre PRP çıkış (F4 74 - DB 6F) ve</span><span class="sxs-lookup"><span data-stu-id="02621-160">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="02621-161">HMAC PRF çıkış (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="02621-161">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="02621-162">CBC modunda şifreleme + HMAC kimlik doğrulama bağlam üstbilgisi algoritmaları uygulamaları Windows CNG veya yönetilen SymmetricAlgorithm ve KeyedHashAlgorithm türleri tarafından sağlanan bağımsız olarak aynı şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="02621-162">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="02621-163">Bu algoritmalar uygulamaları işletim sistemleri arasında farklılık olsa bile aynı içerik üstbilgisi güvenilir bir şekilde oluşturmak farklı işletim sistemlerinde çalışan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="02621-163">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="02621-164">(Pratikte, KeyedHashAlgorithm uygun HMAC olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="02621-164">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="02621-165">Bu herhangi bir anahtarlı karma algoritması türü olabilir.)</span><span class="sxs-lookup"><span data-stu-id="02621-165">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="02621-166">Örnek: 3DES 192 CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="02621-166">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="02621-167">İlk olarak, sağlar (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 192 bitlik ve | K_H | = Belirtilen algoritma başına 160 bit.</span><span class="sxs-lookup"><span data-stu-id="02621-167">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="02621-168">Bu K_E için müşteri adayları A219 =... E2BB ve K_H DC4A =... Aşağıdaki örnekte B464:</span><span class="sxs-lookup"><span data-stu-id="02621-168">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="02621-169">Ardından, Enc_CBC işlem (K_E, IV, "") 3DES-192-IV verilen CBC için = 0 * ve yukarıdaki K_E.</span><span class="sxs-lookup"><span data-stu-id="02621-169">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0* and K_E as above.</span></span>

<span data-ttu-id="02621-170">Sonuç: ABB100F81E53E10E =</span><span class="sxs-lookup"><span data-stu-id="02621-170">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="02621-171">Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA1 için.</span><span class="sxs-lookup"><span data-stu-id="02621-171">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="02621-172">Sonuç: 76EB189B35CF03461DDF877CD9F4B1B4D63A7555 =</span><span class="sxs-lookup"><span data-stu-id="02621-172">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="02621-173">Bu bir parmak izi kimliği doğrulanmış tüm içerik başlık oluşturur aşağıda gösterilen şifreleme algoritması çifti (3DES 192 CBC şifreleme + HMACSHA1 doğrulama):</span><span class="sxs-lookup"><span data-stu-id="02621-173">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="02621-174">Bileşenleri gibi Bölünme:</span><span class="sxs-lookup"><span data-stu-id="02621-174">The components break down as follows:</span></span>

* <span data-ttu-id="02621-175">işaretin (00 00)</span><span class="sxs-lookup"><span data-stu-id="02621-175">the marker (00 00)</span></span>

* <span data-ttu-id="02621-176">Blok şifre anahtar uzunluğu (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="02621-176">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="02621-177">Blok Şifre blok boyutu (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="02621-177">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="02621-178">HMAC anahtar uzunluğu (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="02621-178">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="02621-179">HMAC Özet boyutu (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="02621-179">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="02621-180">Blok şifre PRP çıkış (AB B1 - E1 0E) ve</span><span class="sxs-lookup"><span data-stu-id="02621-180">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="02621-181">HMAC PRF çıkış (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="02621-181">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="02621-182">Galois/sayaç modu şifreleme + kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="02621-182">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="02621-183">İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="02621-183">The context header consists of the following components:</span></span>

* <span data-ttu-id="02621-184">[16 bit] 00 değeri bir işaretçisi 01 "GCM şifreleme + kimlik doğrulaması" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="02621-184">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="02621-185">[32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) simetrik blok şifreleme algoritması.</span><span class="sxs-lookup"><span data-stu-id="02621-185">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="02621-186">[32 bit] Kimliği doğrulanmış şifreleme işlemleri sırasında kullanılan nonce boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="02621-186">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="02621-187">(Sistemimizde için bu nonce boyutta sabittir = 96 bit.)</span><span class="sxs-lookup"><span data-stu-id="02621-187">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="02621-188">[32 bit] Simetrik blok şifreleme algoritmasının blok boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="02621-188">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="02621-189">(GCM için bu blok boyutuyla sabittir 128 bit =.)</span><span class="sxs-lookup"><span data-stu-id="02621-189">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="02621-190">[32 bit] Kimliği doğrulanmış şifreleme işleviyle üretilen kimlik doğrulama etiketi boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="02621-190">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="02621-191">(Sistemimizde için bu etiketi boyutta sabittir 128 bit =.)</span><span class="sxs-lookup"><span data-stu-id="02621-191">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="02621-192">[128 bit] Enc_GCM etiket (K_E, nonce, ""), boş bir dize giriş verilen simetrik blok şifreleme algoritması çıktısını olduğu ve nonce 96 bit tüm sıfır vektör olduğu.</span><span class="sxs-lookup"><span data-stu-id="02621-192">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="02621-193">K_E CBC şifreleme + HMAC kimlik doğrulama senaryosu olduğu gibi aynı mekanizmayı kullanarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="02621-193">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="02621-194">Ancak, burada play'de hiçbir K_H olduğundan, aslında sahibiz | K_H | = 0, ve için algoritma daraltır formun altındaki.</span><span class="sxs-lookup"><span data-stu-id="02621-194">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="02621-195">K_E SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = "")</span><span class="sxs-lookup"><span data-stu-id="02621-195">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="02621-196">Örnek: AES 256 GCM</span><span class="sxs-lookup"><span data-stu-id="02621-196">Example: AES-256-GCM</span></span>

<span data-ttu-id="02621-197">İlk olarak, K_E izin SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 256 bit.</span><span class="sxs-lookup"><span data-stu-id="02621-197">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="02621-198">K_E: 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8 =</span><span class="sxs-lookup"><span data-stu-id="02621-198">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="02621-199">Ardından, Enc_GCM kimlik doğrulaması etiket işlem (K_E, nonce, "") AES-256-nonce verilen GCM için yukarıdaki = 096 ve K_E.</span><span class="sxs-lookup"><span data-stu-id="02621-199">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="02621-200">Sonuç: E7DCCE66DF855A323A6BB7BD7A59BE45 =</span><span class="sxs-lookup"><span data-stu-id="02621-200">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="02621-201">Tüm içerik üst bilgisi üretir:</span><span class="sxs-lookup"><span data-stu-id="02621-201">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="02621-202">Bileşenleri gibi Bölünme:</span><span class="sxs-lookup"><span data-stu-id="02621-202">The components break down as follows:</span></span>

   * <span data-ttu-id="02621-203">işaretin (00 01)</span><span class="sxs-lookup"><span data-stu-id="02621-203">the marker (00 01)</span></span>

   * <span data-ttu-id="02621-204">Blok şifre anahtar uzunluğu (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="02621-204">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="02621-205">nonce boyutu (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="02621-205">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="02621-206">Blok Şifre blok boyutu (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="02621-206">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="02621-207">kimlik doğrulama etiketi boyutu (00 00 00 10) ve</span><span class="sxs-lookup"><span data-stu-id="02621-207">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="02621-208">Blok şifre çalışmasını kimlik doğrulaması etiketi (E7 DC - end).</span><span class="sxs-lookup"><span data-stu-id="02621-208">the authentication tag from running the block cipher (E7 DC - end).</span></span>
