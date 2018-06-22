---
title: ASP.NET Core içerik üstbilgileri
author: rick-anderson
description: ASP.NET Core veri koruması içerik üstbilgileri uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2343e59898c024eba420390d7fb0bce2fc82a895
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274475"
---
# <a name="context-headers-in-aspnet-core"></a>ASP.NET Core içerik üstbilgileri

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>Arka plan ve teorik

Veri koruma sisteminde şifreleme hizmetleri sağlayan bir nesne kimliği doğrulanmış bir "anahtar" anlamına gelir. Her anahtar benzersiz bir kimlik (GUID) tarafından tanımlanır ve onunla taşıyan algoritmik bilgi ve entropic malzeme. Her anahtar taşımak benzersiz entropi, ancak sistem, zorunlu kılamaz ve ayrıca varolan anahtarı anahtar halkası algoritmik bilgilerinin değiştirerek anahtar halkası el ile değişebilir geliştiriciler için hesap ihtiyacımız yöneliktir. Bu gibi durumlarda verilen bizim güvenlik gereksinimlerini elde etmek için veri koruma sisteminde bir kavramı vardır [şifreleme çevikliği](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), güvenli bir şekilde tek bir entropic değer arasında birden çok şifreleme algoritmaları kullanarak izin verir.

Şifreleme çevikliği destekleyen çoğu sistemi yükünün algoritması hakkında bazı tanımlama bilgileri ekleyerek bunu. Algoritma 's OID genellikle iyi bir aday içindir. Ancak, aynı algoritma belirtmek için birden çok yolu vardır içine karşılaştık bir sorun olduğunu: "AES" (CNG) ve yönetilen Aes, AesManaged, AesCryptoServiceProvider, AesCng ve (belirli parametreleri verilir) RijndaelManaged sınıflarıdır tüm aslında aynı tüm bunların doğru OID için bir eşleme korumak şey ve biz gerekir. Bir geliştirici özel bir algoritma (veya hatta başka bir AES uyarlamasını!) sağlamak istiyorsanız, OID bize iletmek gerekir. Bu ek kayıt adım sistem yapılandırması özellikle Acı verici sağlar.

Geri atlama, biz sorun yanlış yönünden yaklaşmakta olduğunu karar. OID, algoritma nedir bildirir, ancak biz aslında bu hakkında önemli değil. Biz tek bir entropic değer güvenli bir şekilde iki farklı algoritmalara kullanmanız gerekiyorsa, bize algoritmaları gerçekte ne olduğunu öğrenmek gerekli değildir. Ne biz gerçekte önem verdiğiniz nasıl davranırlar olur. Tüm makul simetrik blok şifre de güçlü geçici rastgele sıralamaya (PRP) algoritmasıdır: (modu, IV, düz metin zincirleme anahtarı) girişleri düzeltin ve ciphertext çıktı olasılık yayma ile diğer simetrik blok şifre farklı olacaktır aynı giriş verilen algoritması. Benzer şekilde, tüm makul anahtarlı karma işlevi de güçlü bir geçici rastgele işlevi (PRF) olduğunu ve sabit bir giriş kümesi çıktısını son derece diğer anahtarlı karma işlevinden farklı olacaktır.

Bir bağlam başlığını oluşturmak için bu kavramı güçlü PRPs ve PRFs kullanırız. Bu içerik üstbilgisi kararlı bir parmak izi temelde herhangi belirtilen işlem için kullanılan algoritmalar üzerinden çalışır ve veri koruma sistemi tarafından gereken şifreleme çevikliği sağlar. Bu üst yeniden üretilebilir ve daha sonra bir parçası olarak kullanılan [alt anahtar türetme işlem](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). İçerik Üstbilgisi işlemi temel algoritmalarının modlarını bağlı olarak oluşturmak için iki farklı yolu vardır.

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC modunda şifreleme + HMAC kimlik doğrulaması

<a name="data-protection-implementation-context-headers-cbc-components"></a>

İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:

* [16 bit] 00 değeri bir işaretçisi 00 "CBC şifreleme + HMAC kimlik doğrulaması" anlamına gelir.

* [32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) simetrik blok şifreleme algoritması.

* [32 bit] Simetrik blok şifreleme algoritmasının blok boyutu (bayt cinsinden, big endian).

* [32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) HMAC algoritması. (Şu anda anahtar boyutu her zaman Özet boyutu eşleşir.)

* [32 bit] Özet boyutunu (bayt cinsinden, big endian) HMAC algoritması.

* EncCBC (K_E, IV, ""), boş bir dize giriş verilen simetrik blok şifreleme algoritması çıktısını olduğu ve IV tüm sıfır vektör olduğu. K_E yapımı aşağıda açıklanmıştır.

* MAC (K_H, ""), boş bir dize giriş verilen HMAC algoritması çıktısını olduğu. K_H yapımı aşağıda açıklanmıştır.

İdeal olarak, biz K_E ve K_H için tüm sıfır vektörlerinin geçirebilirdiniz. Ancak, burada temel algoritma, tüm sıfır vektör gibi basit veya repeatable bir düzeni kullanarak önleyen (özellikle DES ve 3DES), herhangi bir işlem gerçekleştirmeden önce zayıf anahtarların bulunmadığını kontrol eder durumdan kaçınmak istiyoruz.

Bunun yerine, biz NIST SP800 108 KDF sayaç modunda kullanın (bkz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), sn 5.1) bir sıfır uzunluklu anahtar, etiket ve bağlamı ve temel PRF olarak HMACSHA512. Biz türetilen | K_E | + | K_H | Çıkış, bayt sonra parçalayın sonucu K_E ve K_H kendilerini. Matematiksel, bunu şu şekilde gösterilir.

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Örnek: AES 192 CBC + HMACSHA256

Örnek olarak, burada simetrik blok şifreleme algoritması AES 192 CBC ve doğrulama algoritması HMACSHA256 durumu göz önünde bulundurun. Sistem aşağıdaki adımları kullanarak içerik üstbilgisi oluşturur.

İlk olarak, sağlar (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 192 bitlik ve | K_H | = belirtilen algoritmaları 256 bit. Bu K_E için müşteri adayları 5BB6 =... 21DD ve K_H A04A =... Aşağıdaki örnekte 00A9:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

Ardından, Enc_CBC işlem (K_E, IV, "") AES-192-IV verilen CBC için = 0 * ve yukarıdaki K_E.

Sonuç: F474B1872B3B53E4721DE19C0841DB6F =

Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA256 için.

Sonuç: D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C =

Tüm içerik üst bilgisi üretir:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Parmak izi kimliği doğrulanmış şifreleme algoritması çiftinin (AES 192 CBC şifreleme + HMACSHA256 doğrulama) bu içeriği başlığıdır. Açıklandığı gibi bileşenleri [yukarıda](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) şunlardır:

* işaretin (00 00)

* Blok şifre anahtar uzunluğu (00 00 00 18)

* Blok Şifre blok boyutu (00 00 00 10)

* HMAC anahtar uzunluğu (00 00 00 20)

* HMAC Özet boyutu (00 00 00 20)

* Blok şifre PRP çıkış (F4 74 - DB 6F) ve

* HMAC PRF çıkış (D4 79 - end).

> [!NOTE]
> CBC modunda şifreleme + HMAC kimlik doğrulama bağlam üstbilgisi algoritmaları uygulamaları Windows CNG veya yönetilen SymmetricAlgorithm ve KeyedHashAlgorithm türleri tarafından sağlanan bağımsız olarak aynı şekilde oluşturulur. Bu algoritmalar uygulamaları işletim sistemleri arasında farklılık olsa bile aynı içerik üstbilgisi güvenilir bir şekilde oluşturmak farklı işletim sistemlerinde çalışan uygulamalar sağlar. (Pratikte, KeyedHashAlgorithm uygun HMAC olması gerekmez. Bu herhangi bir anahtarlı karma algoritması türü olabilir.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Örnek: 3DES 192 CBC + HMACSHA1

İlk olarak, sağlar (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 192 bitlik ve | K_H | = Belirtilen algoritma başına 160 bit. Bu K_E için müşteri adayları A219 =... E2BB ve K_H DC4A =... Aşağıdaki örnekte B464:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

Ardından, Enc_CBC işlem (K_E, IV, "") 3DES-192-IV verilen CBC için = 0 * ve yukarıdaki K_E.

Sonuç: ABB100F81E53E10E =

Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA1 için.

Sonuç: 76EB189B35CF03461DDF877CD9F4B1B4D63A7555 =

Bu bir parmak izi kimliği doğrulanmış tüm içerik başlık oluşturur aşağıda gösterilen şifreleme algoritması çifti (3DES 192 CBC şifreleme + HMACSHA1 doğrulama):

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Bileşenleri gibi Bölünme:

* işaretin (00 00)

* Blok şifre anahtar uzunluğu (00 00 00 18)

* Blok Şifre blok boyutu (00 00 00 08)

* HMAC anahtar uzunluğu (00 00 00 14)

* HMAC Özet boyutu (00 00 00 14)

* Blok şifre PRP çıkış (AB B1 - E1 0E) ve

* HMAC PRF çıkış (76 EB - end).

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/sayaç modu şifreleme + kimlik doğrulaması

İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:

* [16 bit] 00 değeri bir işaretçisi 01 "GCM şifreleme + kimlik doğrulaması" anlamına gelir.

* [32 bit] Anahtar uzunluğu (bayt cinsinden, big endian) simetrik blok şifreleme algoritması.

* [32 bit] Kimliği doğrulanmış şifreleme işlemleri sırasında kullanılan nonce boyutu (bayt cinsinden, big endian). (Sistemimizde için bu nonce boyutta sabittir = 96 bit.)

* [32 bit] Simetrik blok şifreleme algoritmasının blok boyutu (bayt cinsinden, big endian). (GCM için bu blok boyutuyla sabittir 128 bit =.)

* [32 bit] Kimliği doğrulanmış şifreleme işleviyle üretilen kimlik doğrulama etiketi boyutu (bayt cinsinden, big endian). (Sistemimizde için bu etiketi boyutta sabittir 128 bit =.)

* [128 bit] Enc_GCM etiket (K_E, nonce, ""), boş bir dize giriş verilen simetrik blok şifreleme algoritması çıktısını olduğu ve nonce 96 bit tüm sıfır vektör olduğu.

K_E CBC şifreleme + HMAC kimlik doğrulama senaryosu olduğu gibi aynı mekanizmayı kullanarak elde edilir. Ancak, burada play'de hiçbir K_H olduğundan, aslında sahibiz | K_H | = 0, ve için algoritma daraltır formun altındaki.

K_E SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = "")

### <a name="example-aes-256-gcm"></a>Örnek: AES 256 GCM

İlk olarak, K_E izin SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiket = "", bağlam = ""), burada | K_E | = 256 bit.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Ardından, Enc_GCM kimlik doğrulaması etiket işlem (K_E, nonce, "") AES-256-nonce verilen GCM için yukarıdaki = 096 ve K_E.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

Tüm içerik üst bilgisi üretir:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Bileşenleri gibi Bölünme:

   * işaretin (00 01)

   * Blok şifre anahtar uzunluğu (00 00 00 20)

   * nonce boyutu (00 00 00 0 C)

   * Blok Şifre blok boyutu (00 00 00 10)

   * kimlik doğrulama etiketi boyutu (00 00 00 10) ve

   * Blok şifre çalışmasını kimlik doğrulaması etiketi (E7 DC - end).
