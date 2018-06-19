---
title: ASP.NET Core içerik üstbilgileri
author: rick-anderson
description: ASP.NET Core veri koruması içerik üstbilgileri uygulama ayrıntılarını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 5ba247a74e11408145e1f6e87c7cfa251c66707f
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077860"
---
# <a name="context-headers-in-aspnet-core"></a><span data-ttu-id="e33aa-103">ASP.NET Core içerik üstbilgileri</span><span class="sxs-lookup"><span data-stu-id="e33aa-103">Context headers in ASP.NET Core</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="e33aa-104">Arka plan ve teorik</span><span class="sxs-lookup"><span data-stu-id="e33aa-104">Background and theory</span></span>

<span data-ttu-id="e33aa-105">Veri koruma sisteminde şifreleme hizmetleri sağlayan bir nesne kimliği doğrulanmış bir "anahtar" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="e33aa-106">Her anahtar benzersiz bir kimlik (GUID) tarafından tanımlanır ve onunla taşıyan algoritmik bilgi ve entropic malzeme.</span><span class="sxs-lookup"><span data-stu-id="e33aa-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="e33aa-107">Her anahtar taşımak benzersiz entropi, ancak sistem, zorunlu kılamaz ve ayrıca varolan anahtarı anahtar halkası algoritmik bilgilerinin değiştirerek anahtar halkası el ile değişebilir geliştiriciler için hesap ihtiyacımız yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="e33aa-108">Bu gibi durumlarda verilen bizim güvenlik gereksinimlerini elde etmek için veri koruma sisteminde bir kavramı vardır [şifreleme çevikliği](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), güvenli bir şekilde tek bir entropic değer arasında birden çok şifreleme algoritmaları kullanarak izin verir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="e33aa-109">Şifreleme çevikliği destekleyen çoğu sistemi yükünün algoritması hakkında bazı tanımlama bilgileri ekleyerek bunu.</span><span class="sxs-lookup"><span data-stu-id="e33aa-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="e33aa-110">Algoritma 's OID genellikle iyi bir aday içindir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="e33aa-111">Ancak, aynı algoritma belirtmek için birden çok yolu vardır içine karşılaştık bir sorun olduğunu: "AES" (CNG) ve yönetilen Aes, AesManaged, AesCryptoServiceProvider, AesCng ve (belirli parametreleri verilir) RijndaelManaged sınıflarıdır tüm aslında aynı tüm bunların doğru OID için bir eşleme korumak şey ve biz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="e33aa-112">Bir geliştirici özel bir algoritma (veya hatta başka bir AES uyarlamasını!) sağlamak istiyorsanız, OID bize iletmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="e33aa-113">Bu ek kayıt adım sistem yapılandırması özellikle Acı verici sağlar.</span><span class="sxs-lookup"><span data-stu-id="e33aa-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="e33aa-114">Geri atlama, biz sorun yanlış yönünden yaklaşmakta olduğunu karar.</span><span class="sxs-lookup"><span data-stu-id="e33aa-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="e33aa-115">OID, algoritma nedir bildirir, ancak biz aslında bu hakkında önemli değil.</span><span class="sxs-lookup"><span data-stu-id="e33aa-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="e33aa-116">Biz tek bir entropic değer güvenli bir şekilde iki farklı algoritmalara kullanmanız gerekiyorsa, bize algoritmaları gerçekte ne olduğunu öğrenmek gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="e33aa-117">Ne biz gerçekte önem verdiğiniz nasıl davranırlar olur.</span><span class="sxs-lookup"><span data-stu-id="e33aa-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="e33aa-118">Tüm makul simetrik blok şifre de güçlü geçici rastgele sıralamaya (PRP) algoritmasıdır: (modu, IV, düz metin zincirleme anahtarı) girişleri düzeltin ve ciphertext çıktı olasılık yayma ile diğer simetrik blok şifre farklı olacaktır aynı giriş verilen algoritması.</span><span class="sxs-lookup"><span data-stu-id="e33aa-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="e33aa-119">Benzer şekilde, tüm makul anahtarlı karma işlevi de güçlü bir geçici rastgele işlevi (PRF) olduğunu ve sabit bir giriş kümesi çıktısını son derece diğer anahtarlı karma işlevinden farklı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e33aa-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="e33aa-120">Bir bağlam başlığını oluşturmak için bu kavramı güçlü PRPs ve PRFs kullanırız.</span><span class="sxs-lookup"><span data-stu-id="e33aa-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="e33aa-121">Bu içerik üstbilgisi kararlı bir parmak izi temelde herhangi belirtilen işlem için kullanılan algoritmalar üzerinden çalışır ve veri koruma sistemi tarafından gereken şifreleme çevikliği sağlar.</span><span class="sxs-lookup"><span data-stu-id="e33aa-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="e33aa-122">Bu üst yeniden üretilebilir ve daha sonra bir parçası olarak kullanılan [alt anahtar türetme işlem](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="e33aa-122">This header is reproducible and is used later as part of the [subkey derivation process](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="e33aa-123">İçerik Üstbilgisi işlemi temel algoritmalarının modlarını bağlı olarak oluşturmak için iki farklı yolu vardır.</span><span class="sxs-lookup"><span data-stu-id="e33aa-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="e33aa-124">CBC modunda şifreleme + HMAC kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e33aa-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="e33aa-125">İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="e33aa-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="e33aa-126">[16 bit] 00 değeri bir işaretçisi 00 "CBC şifreleme + HMAC kimlik doğrulaması" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="e33aa-127">[32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) simetrik blok şifreleme algoritması.</span><span class="sxs-lookup"><span data-stu-id="e33aa-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e33aa-128">[32 bit] Simetrik blok şifreleme algoritmasının blok boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="e33aa-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e33aa-129">[32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) HMAC algoritması.</span><span class="sxs-lookup"><span data-stu-id="e33aa-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="e33aa-130">(Şu anda anahtar boyutu her zaman Özet boyutu eşleşir.)</span><span class="sxs-lookup"><span data-stu-id="e33aa-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="e33aa-131">[32 bit] Özet boyutunu (bayt cinsinden, big endian) HMAC algoritması.</span><span class="sxs-lookup"><span data-stu-id="e33aa-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="e33aa-132">EncCBC (K_E, IV, ""), boş bir dize giriş verilen simetrik blok şifreleme algoritması çıktısını olduğu ve IV tüm sıfır vektör olduğu.</span><span class="sxs-lookup"><span data-stu-id="e33aa-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="e33aa-133">K_E yapımı aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e33aa-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="e33aa-134">MAC (K_H, ""), boş bir dize giriş verilen HMAC algoritması çıktısını olduğu.</span><span class="sxs-lookup"><span data-stu-id="e33aa-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="e33aa-135">K_H yapımı aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e33aa-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="e33aa-136">İdeal olarak, biz K_E ve K_H için tüm sıfır vektörlerinin geçirebilirdiniz.</span><span class="sxs-lookup"><span data-stu-id="e33aa-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="e33aa-137">Ancak, burada temel algoritma, tüm sıfır vektör gibi basit veya repeatable bir düzeni kullanarak önleyen (özellikle DES ve 3DES), herhangi bir işlem gerçekleştirmeden önce zayıf anahtarların bulunmadığını kontrol eder durumdan kaçınmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="e33aa-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="e33aa-138">Bunun yerine, biz NIST SP800 108 KDF sayaç modunda kullanın (bkz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sn 5.1) bir sıfır uzunluklu anahtar, etiket ve bağlamı ve temel PRF olarak HMACSHA512.</span><span class="sxs-lookup"><span data-stu-id="e33aa-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="e33aa-139">Biz türetilen | K_E | + | K_H | Çıkış, bayt sonra parçalayın sonucu K_E ve K_H kendilerini.</span><span class="sxs-lookup"><span data-stu-id="e33aa-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="e33aa-140">Matematiksel, bunu şu şekilde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="e33aa-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="e33aa-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="e33aa-142">Örnek: AES 192 CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="e33aa-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="e33aa-143">Örnek olarak, burada simetrik blok şifreleme algoritması AES 192 CBC ve doğrulama algoritması HMACSHA256 durumu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="e33aa-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="e33aa-144">Sistem aşağıdaki adımları kullanarak içerik üstbilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e33aa-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="e33aa-145">İlk olarak, sağlar (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 192 bitlik ve | K_H | = belirtilen algoritmaları 256 bit.</span><span class="sxs-lookup"><span data-stu-id="e33aa-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="e33aa-146">Bu K_E için müşteri adayları 5BB6 =... 21DD ve K_H A04A =... Aşağıdaki örnekte 00A9:</span><span class="sxs-lookup"><span data-stu-id="e33aa-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="e33aa-147">Ardından, Enc_CBC işlem (K_E, IV, "") AES-192-IV verilen CBC için = 0 \* ve yukarıdaki K_E.</span><span class="sxs-lookup"><span data-stu-id="e33aa-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="e33aa-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="e33aa-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="e33aa-149">Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA256 için.</span><span class="sxs-lookup"><span data-stu-id="e33aa-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="e33aa-150">Sonuç: D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C =</span><span class="sxs-lookup"><span data-stu-id="e33aa-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="e33aa-151">Tüm içerik üst bilgisi üretir:</span><span class="sxs-lookup"><span data-stu-id="e33aa-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="e33aa-152">Parmak izi kimliği doğrulanmış şifreleme algoritması çiftinin (AES 192 CBC şifreleme + HMACSHA256 doğrulama) bu içeriği başlığıdır.</span><span class="sxs-lookup"><span data-stu-id="e33aa-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="e33aa-153">Açıklandığı gibi bileşenleri [yukarıda](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e33aa-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="e33aa-154">işaretin (00 00)</span><span class="sxs-lookup"><span data-stu-id="e33aa-154">the marker (00 00)</span></span>

* <span data-ttu-id="e33aa-155">Blok şifre anahtar uzunluğu (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="e33aa-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="e33aa-156">Blok Şifre blok boyutu (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="e33aa-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="e33aa-157">HMAC anahtar uzunluğu (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e33aa-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="e33aa-158">HMAC Özet boyutu (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e33aa-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="e33aa-159">Blok şifre PRP çıkış (F4 74 - DB 6F) ve</span><span class="sxs-lookup"><span data-stu-id="e33aa-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="e33aa-160">HMAC PRF çıkış (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="e33aa-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="e33aa-161">CBC modunda şifreleme + HMAC kimlik doğrulama bağlam üstbilgisi algoritmaları uygulamaları Windows CNG veya yönetilen SymmetricAlgorithm ve KeyedHashAlgorithm türleri tarafından sağlanan bağımsız olarak aynı şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e33aa-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="e33aa-162">Bu algoritmalar uygulamaları işletim sistemleri arasında farklılık olsa bile aynı içerik üstbilgisi güvenilir bir şekilde oluşturmak farklı işletim sistemlerinde çalışan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="e33aa-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="e33aa-163">(Pratikte, KeyedHashAlgorithm uygun HMAC olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e33aa-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="e33aa-164">Bu herhangi bir anahtarlı karma algoritması türü olabilir.)</span><span class="sxs-lookup"><span data-stu-id="e33aa-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="e33aa-165">Örnek: 3DES 192 CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="e33aa-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="e33aa-166">İlk olarak, sağlar (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 192 bitlik ve | K_H | = Belirtilen algoritma başına 160 bit.</span><span class="sxs-lookup"><span data-stu-id="e33aa-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="e33aa-167">Bu K_E için müşteri adayları A219 =... E2BB ve K_H DC4A =... Aşağıdaki örnekte B464:</span><span class="sxs-lookup"><span data-stu-id="e33aa-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="e33aa-168">Ardından, Enc_CBC işlem (K_E, IV, "") 3DES-192-IV verilen CBC için = 0 \* ve yukarıdaki K_E.</span><span class="sxs-lookup"><span data-stu-id="e33aa-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="e33aa-169">result := ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="e33aa-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="e33aa-170">Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA1 için.</span><span class="sxs-lookup"><span data-stu-id="e33aa-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="e33aa-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="e33aa-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="e33aa-172">Bu bir parmak izi kimliği doğrulanmış tüm içerik başlık oluşturur aşağıda gösterilen şifreleme algoritması çifti (3DES 192 CBC şifreleme + HMACSHA1 doğrulama):</span><span class="sxs-lookup"><span data-stu-id="e33aa-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="e33aa-173">Bileşenleri gibi Bölünme:</span><span class="sxs-lookup"><span data-stu-id="e33aa-173">The components break down as follows:</span></span>

* <span data-ttu-id="e33aa-174">işaretin (00 00)</span><span class="sxs-lookup"><span data-stu-id="e33aa-174">the marker (00 00)</span></span>

* <span data-ttu-id="e33aa-175">Blok şifre anahtar uzunluğu (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="e33aa-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="e33aa-176">Blok Şifre blok boyutu (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="e33aa-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="e33aa-177">HMAC anahtar uzunluğu (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="e33aa-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="e33aa-178">HMAC Özet boyutu (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="e33aa-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="e33aa-179">Blok şifre PRP çıkış (AB B1 - E1 0E) ve</span><span class="sxs-lookup"><span data-stu-id="e33aa-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="e33aa-180">HMAC PRF çıkış (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="e33aa-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="e33aa-181">Galois/sayaç modu şifreleme + kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="e33aa-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="e33aa-182">İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:</span><span class="sxs-lookup"><span data-stu-id="e33aa-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="e33aa-183">[16 bit] 00 değeri bir işaretçisi 01 "GCM şifreleme + kimlik doğrulaması" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="e33aa-184">[32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) simetrik blok şifreleme algoritması.</span><span class="sxs-lookup"><span data-stu-id="e33aa-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="e33aa-185">[32 bit] Kimliği doğrulanmış şifreleme işlemleri sırasında kullanılan nonce boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="e33aa-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="e33aa-186">(Sistemimizde için bu nonce boyutta sabittir = 96 bit.)</span><span class="sxs-lookup"><span data-stu-id="e33aa-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="e33aa-187">[32 bit] Simetrik blok şifreleme algoritmasının blok boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="e33aa-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="e33aa-188">(GCM için bu blok boyutuyla sabittir 128 bit =.)</span><span class="sxs-lookup"><span data-stu-id="e33aa-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="e33aa-189">[32 bit] Kimliği doğrulanmış şifreleme işleviyle üretilen kimlik doğrulama etiketi boyutu (bayt cinsinden, big endian).</span><span class="sxs-lookup"><span data-stu-id="e33aa-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="e33aa-190">(Sistemimizde için bu etiketi boyutta sabittir 128 bit =.)</span><span class="sxs-lookup"><span data-stu-id="e33aa-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="e33aa-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span><span class="sxs-lookup"><span data-stu-id="e33aa-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="e33aa-192">K_E CBC şifreleme + HMAC kimlik doğrulama senaryosu olduğu gibi aynı mekanizmayı kullanarak elde edilir.</span><span class="sxs-lookup"><span data-stu-id="e33aa-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="e33aa-193">Ancak, burada play'de hiçbir K_H olduğundan, aslında sahibiz | K_H | = 0, ve için algoritma daraltır formun altındaki.</span><span class="sxs-lookup"><span data-stu-id="e33aa-193">However, since there's no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="e33aa-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span><span class="sxs-lookup"><span data-stu-id="e33aa-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="e33aa-195">Örnek: AES 256 GCM</span><span class="sxs-lookup"><span data-stu-id="e33aa-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="e33aa-196">İlk olarak, K_E izin SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 256 bit.</span><span class="sxs-lookup"><span data-stu-id="e33aa-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="e33aa-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="e33aa-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="e33aa-198">Ardından, Enc_GCM kimlik doğrulaması etiket işlem (K_E, nonce, "") AES-256-nonce verilen GCM için yukarıdaki = 096 ve K_E.</span><span class="sxs-lookup"><span data-stu-id="e33aa-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="e33aa-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="e33aa-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="e33aa-200">Tüm içerik üst bilgisi üretir:</span><span class="sxs-lookup"><span data-stu-id="e33aa-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="e33aa-201">Bileşenleri gibi Bölünme:</span><span class="sxs-lookup"><span data-stu-id="e33aa-201">The components break down as follows:</span></span>

   * <span data-ttu-id="e33aa-202">işaretin (00 01)</span><span class="sxs-lookup"><span data-stu-id="e33aa-202">the marker (00 01)</span></span>

   * <span data-ttu-id="e33aa-203">Blok şifre anahtar uzunluğu (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="e33aa-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="e33aa-204">nonce boyutu (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="e33aa-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="e33aa-205">Blok Şifre blok boyutu (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="e33aa-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="e33aa-206">kimlik doğrulama etiketi boyutu (00 00 00 10) ve</span><span class="sxs-lookup"><span data-stu-id="e33aa-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="e33aa-207">Blok şifre çalışmasını kimlik doğrulaması etiketi (E7 DC - end).</span><span class="sxs-lookup"><span data-stu-id="e33aa-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
