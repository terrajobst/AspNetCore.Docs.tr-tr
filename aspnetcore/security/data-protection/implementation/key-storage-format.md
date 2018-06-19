---
title: ASP.NET Core anahtar depolama biçimi
author: rick-anderson
description: ASP.NET Core veri koruması anahtar depolama biçimi uygulama ayrıntılarını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 1a5912f246708355e6677c60034d982d053c3938
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153579"
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core anahtar depolama biçimi

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

Bir <encryptedSecret> şifrelenmiş gizli anahtar malzemesi içeren öğesi mevcut olması durumunda [REST gizli şifreleme etkinleştirilmiştir](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest). Öznitelik decryptorType IXmlDecryptor uygulayan bir tür bütünleştirilmiş kod tam adı olacaktır. Bu tür iç okumak için sorumlu olduğu <encryptedKey> öğesi ve özgün düz metin kurtarmak için şifre çözme.

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
