---
title: "Anahtar depolama biçimi"
author: tdykstra
description: "Bu belgede ASP.NET Core veri koruma anahtarı depolama alanı biçimi, uygulama ayrıntıları açıklanmaktadır."
keywords: ASP.NET Core, veri koruma, anahtar depolama
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4ebad05f7d55e954463ce5e277b419a7d6f773c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="key-storage-format"></a>Anahtar depolama biçimi

<a name="data-protection-implementation-key-storage-format"></a>

Nesneleri XML gösterimi bekleyen depolanır. Anahtar depolama alanı için varsayılan % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\ dizinidir.

## <a name="the-key-element"></a>\<Anahtar > öğesi

Üst düzey nesnelerindeki anahtar deposu olarak anahtarları mevcut. Kurala göre filename anahtarlara sahip **anahtarı-{GUID} .xml**, {GUID} anahtarının kimliğidir. Her bir dosya tek bir anahtar içerir. Dosya biçimi aşağıdaki gibidir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

\<Anahtar > öğesi aşağıdaki öznitelikler ve alt öğeleri içerir:

* Anahtar kimliği. Bu değer yetkili olarak kabul edilir; yalnızca bir nicety İnsan okunabilirlik için dosya adıdır.

* Sürümü \<anahtar > öğesi, şu anda 1'den sabit.

* Anahtarın oluşturma, etkinleştirme ve sona erme tarihleri.

* A \<tanımlayıcısı > Bu anahtarı içinde yer alan kimliği doğrulanmış şifreleme uygulaması hakkında bilgi içeren öğe.

Yukarıdaki örnekte anahtarın kimliği {80732141-ec8f-4b80-af9c-c4d2d1ff8901} olduğunda, onu oluşturuldu ve 19 Mart 2015 tarihinde etkinleştirildi ve 90 gün ömrü. (Bazen etkinleştirme tarihi biraz önce bu örnekte olduğu gibi oluşturma tarihi olabilir. API'ler nasıl çalışır ve uygulamada zararsız nEtki nedeni budur.)

## <a name="the-descriptor-element"></a>\<Tanımlayıcısı > öğesi

Dış \<tanımlayıcısı > öğesi IAuthenticatedEncryptorDescriptorDeserializer uygulayan bir tür bütünleştirilmiş kod tam adı bir öznitelik deserializerType içerir. Bu tür iç okumak için sorumlu olduğu \<tanımlayıcısı > öğesi ve içinde yer alan bilgileri ayrıştırma.

Belirli biçimi \<tanımlayıcısı > öğesi anahtarının kapsüllenmiş kimliği doğrulanmış Şifreleyici uygulama bağlıdır ve her seri durumdan çıkarıcının türü biraz farklı bir biçim bu için bekliyor. Genel olarak, bu öğe algoritmik bilgilerini yine de içerecektir (adları olan OID, türleri veya benzer) ve gizli anahtar malzemesi. Yukarıdaki örnekte, bu anahtarı AES 256 CBC şifreleme + HMACSHA256 doğrulama kaydırılacağını tanımlayıcısını belirtir.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > öğesi

Bir <encryptedSecret> şifrelenmiş gizli anahtar malzemesi içeren öğesi mevcut olması durumunda [REST gizli şifreleme etkinleştirilmiştir](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest). Öznitelik decryptorType IXmlDecryptor uygulayan bir tür bütünleştirilmiş kod tam adı olacaktır. Bu tür iç okumak için sorumlu olduğu <encryptedKey> öğesi ve özgün düz metin kurtarmak için şifre çözme.

İle \<tanımlayıcısı >, belirli biçimi <encryptedSecret> kullanımda çalışmıyorken şifreleme mekanizması öğesi bağlıdır. Yukarıdaki örnekte, ana anahtar yorum Windows DPAPI kullanılarak şifrelenir.

## <a name="the-revocation-element"></a>\<İptal > öğesi

Üst düzey nesnelerindeki anahtar deposu olarak iptalleri mevcut. Kurala göre iptalleri filename sahip **iptal-{zaman damgası} .xml** (için tüm anahtarları, belirli bir tarihten önce iptal etme) veya **iptal-{GUID} .xml** (için belirli bir anahtarı iptal etme). Tek bir her dosyayı içeren \<iptal > öğesi.

Dosya içeriği için iptalleri tek tek anahtarların olacaktır olarak aşağıda.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

Bu durumda, yalnızca belirtilen anahtarı iptal edilir. Anahtar kimliği ise "*", ancak olarak aşağıdaki örnekte, tüm anahtarları, oluşturulma tarihi olan belirtilen iptal tarihinden önce iptal edilir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Neden > öğesi sistem tarafından hiçbir zaman okuyun. Bu kullanıcı tarafından okunabilen bir iptal nedeni depolamak için yalnızca uygun bir yerdir.
