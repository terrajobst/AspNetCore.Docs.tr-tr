---
title: ASP.NET core'da bağlam üst bilgileri
author: rick-anderson
description: ASP.NET Core veri koruma bağlam üst bilgileri uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2b8fd594672bf623d38bfae90d05a984f92ce6a3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208375"
---
# <a name="context-headers-in-aspnet-core"></a>ASP.NET core'da bağlam üst bilgileri

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>Arka plan ve teorik

Veri koruma sisteminde, şifreleme hizmetleri sağlayan bir nesne kimliği doğrulanmış bir "anahtarını" anlamına gelir. Her anahtar benzersiz bir kimlik (GUID) tarafından tanımlanır ve onunla taşıyan algoritmik bilgi ve entropic malzeme. Her anahtarın benzersiz entropi taşıyan ancak sistem, zorunlu kılamaz ve ayrıca anahtar halkası, el ile mevcut bir anahtarı anahtar halkası, algoritmik bilgilerini değiştirerek değişebilir geliştiriciler için hesap gerekiyor yöneliktir. Bu gibi durumlarda verilen bizim güvenlik gereksinimlerini karşılamak için veri koruma sisteminde kavramı vardır [şifreleme çevikliği](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), tek bir entropic değer arasında birden fazla şifreleme algoritmaları kullanarak güvenli bir şekilde izin verir.

Şifreleme çevikliği destekleyen sistemlerinin çoğu algoritma yükü içinde ilgili bazı tanımlama bilgilerini ekleyerek bunu. Algoritma 's genellikle Bunun iyi bir aday oıd'dir. Ancak, aynı algoritmayı belirtmek için birden çok yolu vardır içine karşılaştık sorunlardan biri olan: "AES" (CNG) ve yönetilen Aes, AesManaged AesCryptoServiceProvider, AesCng ve RijndaelManaged (belirli parametreleri verilir) tüm aslında aynı sınıflardır ve tüm bunların doğru OID için bir eşleme sağlamak gerekir. Özel bir algoritma (veya hatta başka bir uygulama AES!) sağlamak bir geliştirici istediyseniz, OID bize bildirmek gerekir. Bu ek kayıt adımı sistem yapılandırması özellikle sorunlu hale getirir.

Geri adım atma, biz sorun yanlış yönünden yaklaşmakta, verdik. OID, algoritma nedir bildirir, ancak biz aslında bu hakkında düşünmeniz gerekmez. Tek bir entropic değer güvenli bir şekilde iki farklı algoritmalar kullanmak ihtiyacımız olursa bize algoritmalar gerçekten ne olduğunu öğrenmek gerekli değildir. Ne biz gerçekten çok önem verdiğiniz davranışları olur. Herhangi bir simetrik makul bir blok şifreleme algoritmasını da güçlü bir sözde rastgele permütasyon (PRP) olan: (modu, IV, düz metin zincirleme anahtarı) girişleri düzeltmek ve olasılık aşırı yüklenilmesini ile ciphertext çıkış başka bir simetrik blok şifreleme farklı olacaktır aynı girişlere algoritması. Benzer şekilde, tüm makul anahtarlı karma işlev de güçlü bir sözde rastgele işlevi (PRF), ve sabit bir giriş kümesi çıktısını çok diğer anahtarlı karma işlevinden farklı olacaktır.

Bir bağlam başlığını oluşturmak için bu kavramı güçlü PRPs ve PRFs kullanıyoruz. Bu bağlam üst bilgi kararlı bir parmak izi temelde herhangi bir işlem için kullanılan algoritmalar üzerinden çalışır ve veri koruma sistemi tarafından gereken şifreleme çevikliği sağlar. Bu üst bilgiyi yeniden üretilebilen ve daha sonra bir parçası olarak kullanılan [alt anahtarını türetme işlem](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Temel alınan algoritmaları işlem moduna bağlı olarak bağlam üstbilgiyi oluşturmak için iki farklı yolu vardır.

## <a name="cbc-mode-encryption--hmac-authentication"></a>CBC modunda şifreleme + HMAC kimlik doğrulaması

<a name="data-protection-implementation-context-headers-cbc-components"></a>

İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:

* [16 bit] ' % S'değeri 00 bir işaretçidir 00 "CBC şifreleme + HMAC kimlik doğrulama" anlamına gelir.

* [32 bit] Blok simetrik şifreleme algoritması anahtarı uzunluğu (bayt cinsinden, büyük-endian).

* [32 bit] Blok boyutu (bayt cinsinden, büyük-endian) blok simetrik şifreleme algoritması.

* [32 bit] HMAC algoritmanın anahtar uzunluğu (bayt cinsinden, büyük-endian). (Şu anda anahtar boyutu her zaman Özet boyut eşleşir.)

* [32 bit] HMAC algoritmasının Özet boyutu (bayt cinsinden, büyük-endian).

* EncCBC (K_E, IV, ""), çıkış boş dize girişi verilen simetrik blok şifreleme algoritması olan ve IV tüm sıfır vektör olduğu. K_E oluşumu aşağıda açıklanmıştır.

* MAC (K_H, ""), bir boş dize girişi verilen HMAC algoritması çıktısı gibidir. K_H oluşumu aşağıda açıklanmıştır.

K_E ve K_H için tüm sıfır vektörleri ideal olarak, geçirebiliriz. Bununla birlikte, burada temel algoritma, tüm sıfır vektör gibi basit veya tekrarlanabilir bir deseni kullanılarak ışığının (özellikle DES ve 3DES), herhangi bir işlem gerçekleştirmeden önce bulunup bulunmadığını zayıf anahtarları kontrol eder önlemek istiyoruz.

Bunun yerine, biz NIST SP800 108 KDF sayacı modunda kullanın (bkz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) sıfır uzunluklu anahtar, etiket ve bağlam ve temel alınan PRF olarak HMACSHA512. Biz türetilen | K_E | + | K_H | Çıktı, bayt ardından ayırmak sonucu K_E ve K_H kendilerini. Matematiksel olarak, bunu şu şekilde gösterilir.

( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Örnek: AES 192 CBC + HMACSHA256

Örneğin, burada blok simetrik şifreleme algoritması AES 192 CBC ve doğrulama algoritması HMACSHA256 bir durum düşünün. Sistem aşağıdaki adımları kullanarak içerik üstbilgisi oluşturur.

İlk olarak, izin (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = key = "", etiketi = "", bağlam = ""), burada | K_E | = 192 bitleri ve | K_H | Belirtilen algoritma başına 256 bit =. Bu müşteri adayları için K_E 5BB6 =... 21DD ve K_H A04A =... Aşağıdaki örnekte 00A9:

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

Bu, tam bağlam üst bilgisi oluşturur:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Parmak izi kimliği doğrulanmış şifreleme algoritması çifti (AES 192 CBC şifreleme + HMACSHA256 doğrulama) bu içeriği başlığıdır. Açıklandığı bileşenleri [yukarıda](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) şunlardır:

* işaretin (00 00)

* Blok şifreleme anahtarı uzunluğu (00 00 00 18)

* Blok Şifre blok boyutu (00 00 00 10)

* HMAC anahtar uzunluğu (00 00 00 20)

* HMAC Özet boyut (00 00 00 20)

* Blok şifreleme PRP çıkış (F4 74 - DB 6F) ve

* HMAC PRF çıkış (D4 79 - bitiş).

> [!NOTE]
> CBC modunda şifreleme + HMAC kimlik doğrulama bağlam üstbilgisi algoritmaları uygulamaları Windows CNG veya yönetilen SymmetricAlgorithm ve KeyedHashAlgorithm türleri tarafından sağlanan bağımsız olarak aynı şekilde oluşturulur. Bu algoritmalar uygulamaları işletim sistemleri arasında farklılık olsa bile güvenilir bir şekilde aynı bağlam üst bilgisi üretmek farklı işletim sistemlerinde çalışan uygulamalar sağlar. (Uygulamada KeyedHashAlgorithm uygun bir HMAC olmak zorunda değildir. Herhangi bir anahtarlı karma algoritması türü olabilir.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Örnek: 3DES-192-CBC + HMACSHA1

İlk olarak, izin (K_E || K_H) SP800_108_CTR = (prf HMACSHA512, = key = "", etiketi = "", bağlam = ""), burada | K_E | = 192 bitleri ve | K_H | Belirtilen algoritma başına 160 bit =. Bu müşteri adayları için K_E A219 =... E2BB ve K_H DC4A =... Aşağıdaki örnekte B464:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

Ardından, Enc_CBC işlem (K_E, IV, "") 3DES-192-IV verilen CBC için = 0 * ve yukarıdaki K_E.

Sonuç: ABB100F81E53E10E =

Ardından, MAC işlem (K_H, "") K_H yukarıdaki verilen HMACSHA1 için.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Bu, kimliği doğrulanmış bir parmak izi olan tam içerik üstbilgisi oluşturur aşağıda gösterilen şifreleme algoritması çifti (3DES 192 CBC şifreleme + HMACSHA1 doğrulama):

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Bileşenleri gibi parçalara ayırın:

* işaretin (00 00)

* Blok şifreleme anahtarı uzunluğu (00 00 00 18)

* Blok Şifre blok boyutu (00 00 00 08)

* HMAC anahtar uzunluğu (00 00 00 14)

* HMAC Özet boyut (00 00 00 14)

* Blok şifreleme PRP çıkış (AB B1 - E1 0E) ve

* HMAC PRF çıkış (76 EB - bitiş).

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/sayacı modu şifreleme + kimlik doğrulaması

İçerik üstbilgisi aşağıdaki bileşenlerden oluşur:

* [16 bit] ' % S'değeri 00 bir işaretçidir 01, "GCM şifreleme + kimlik doğrulama" anlamına gelir.

* [32 bit] Blok simetrik şifreleme algoritması anahtarı uzunluğu (bayt cinsinden, büyük-endian).

* [32 bit] Kimliği doğrulanmış şifreleme işlemleri sırasında kullanılan nonce boyutu (bayt cinsinden, büyük-endian). (Sistemimiz için bu nonce boyutunda sabit = 96 bit.)

* [32 bit] Blok boyutu (bayt cinsinden, büyük-endian) blok simetrik şifreleme algoritması. (GCM için bu blok boyutunda sabit = 128 bit.)

* [32 bit] Kimliği doğrulanmış şifreleme işleviyle üretilen kimlik doğrulaması etiketi boyutu (bayt cinsinden, büyük-endian). (Sistemimiz için bu etiketi boyutunda sabit = 128 bit.)

* [128 bit] Enc_GCM etiketi (K_E nonce, ""), çıkış boş dize girişi verilen simetrik blok şifreleme algoritması olan ve nonce 96 bit tüm sıfır vektör olduğu.

K_E CBC şifreleme + kimlik doğrulama senaryosu HMAC olduğu gibi aynı mekanizması kullanılarak elde edilir. Bununla birlikte, burada oyunda hiçbir K_H olduğundan, temelde sahibiz | K_H | 0 = için algoritma daraltılır formun altındaki.

K_E SP800_108_CTR = (prf HMACSHA512, = key = "", etiketi = "", bağlam = "")

### <a name="example-aes-256-gcm"></a>Örnek: AES-256-GCM

İlk olarak, K_E izin SP800_108_CTR = (prf HMACSHA512, = anahtar = "", etiketi = "", bağlam = ""), burada | K_E | = 256 bit.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Ardından, kimlik doğrulaması etiketi Enc_GCM, işlem (K_E nonce, "") AES-256-nonce verilmiş GCM için yukarıdaki = 096 ve K_E.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

Bu, tam bağlam üst bilgisi oluşturur:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Bileşenleri gibi parçalara ayırın:

* işaretin (00 01)

* Blok şifreleme anahtarı uzunluğu (00 00 00 20)

* nonce boyutu (00 00 00 0 C)

* Blok Şifre blok boyutu (00 00 00 10)

* kimlik doğrulaması etiket boyutu (00 00 00 10) ve

* Blok şifreleme çalışmasını kimlik doğrulaması etiketi (DC E7 - bitiş).
