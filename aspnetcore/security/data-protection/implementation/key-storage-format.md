---
title: ASP.NET core'da anahtar depolama biçimi
author: rick-anderson
description: ASP.NET Core veri koruma anahtar depolama biçimi uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: bca19ad001dd20b5d02ae5470f7d928082496037
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219283"
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET core'da anahtar depolama biçimi

<a name="data-protection-implementation-key-storage-format"></a>

Nesneleri, bekleyen veri olarak XML gösterimi depolanır. Anahtar depolama alanı için varsayılan % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\ dizindir.

## <a name="the-key-element"></a>\<Anahtarı > öğesi

Anahtar deposu en üst düzey nesneleri olarak anahtarları mevcut. Kural gereği, dosya adı anahtarlara sahip **anahtarı-{GUID} .xml**, {GUID} anahtarının kimliğidir. Her bir dosya, tek bir anahtar içeriyor. Dosya biçimi aşağıdaki gibidir.

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

\<Anahtarı > öğesi, aşağıdaki öznitelikler ve alt öğeleri içerir:

* Anahtar kimliği. Bu değer, yetkili olarak kabul edilir; yalnızca bir nicety okunabilirlik için dosya adıdır.

* Sürümü \<anahtarı > öğesinde, şu anda 1 sabit.

* Anahtarın oluşturulması, etkinleştirme ve sona erme tarihleri.

* A \<tanımlayıcısı > Bu anahtarı içinde yer alan kimliği doğrulanmış şifreleme uygulama hakkında bilgi içeren öğe.

Yukarıdaki örnekte, {80732141-ec8f-4b80-af9c-c4d2d1ff8901} anahtarının kimliğidir, onu oluşturuldu ve 19 Mart 2015'te etkinleştirilen ve 90 gün ömrü vardır. (Bazen etkinleştirme tarihi biraz önce bu örnekte olduğu gibi oluşturma tarihi olabilir. Bu API'leri nasıl çalışır ve uygulamada zararsızdır bir nEtki kaynaklanır.)

## <a name="the-descriptor-element"></a>\<Tanımlayıcısı > öğesi

Dış \<tanımlayıcısı > öğesi IAuthenticatedEncryptorDescriptorDeserializer uygulayan bir tür bütünleştirilmiş kodla nitelenen adı olan bir öznitelik deserializerType içerir. Bu tür, iç okumak için sorumlu \<tanımlayıcısı > öğesi ve içinde yer alan bilgileri ayrıştırılıyor.

Belirli biçimi \<tanımlayıcısı > öğesi kapsüllenmiş anahtarı tarafından kimliği doğrulanmış bir Şifreleyici uygulama bağlıdır ve her bir seri durumdan çıkarıcının türü biraz farklı bir biçim bunun için bekliyor. Genel olarak, bu öğe algoritmik bilgilerini yine de içerecektir (adları, türleri, OID veya benzer) ve gizli anahtar malzemesi. Yukarıdaki örnekte, bu anahtarı AES 256 CBC şifreleme + HMACSHA256 doğrulama sarmalar tanımlayıcısını belirtir.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > öğesi

Bir **&lt;encryptedSecret&gt;** şifrelenmiş gizli anahtar malzemesi içeren öğe mevcut olması durumunda [gizli anahtarlarının, bekleyen veri şifrelemesi etkin](xref:security/data-protection/implementation/key-encryption-at-rest). Öznitelik `decryptorType` uygulayan bir tür bütünleştirilmiş kodla nitelenen adı [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor). Bu tür, iç okumak için sorumlu **&lt;encryptedKey&gt;** öğesi ve özgün düz metin kurtarmak için şifre çözme.

Olduğu gibi \<tanımlayıcısı >, belirli biçimi <encryptedSecret> öğe kullanımda bekleyen şifreleme mekanizması bağlıdır. Yukarıdaki örnekte, ana anahtarı açıklama Windows DPAPI kullanılarak şifrelenir.

## <a name="the-revocation-element"></a>\<İptal > öğesi

Geri alma işlemleri, anahtar deposu en üst düzey nesneleri olarak mevcut. Kural gereği, dosya adını geri alma işlemleri sahip **iptal-{zaman damgası} .xml** (için belirli bir tarihten önce tüm anahtarlar iptal) veya **iptal-{GUID} .xml** (için belirli bir anahtarı iptal etme). Her dosyada tek bir \<iptal > öğesi.

Dosya içeriği için geri alma işlemleri ayrı ayrı anahtarların olacak şekilde aşağıda.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

Bu durumda, yalnızca belirtilen anahtarı iptal edilir. Anahtar kimliği ise "*", ancak olarak aşağıdaki örnekte, tüm anahtarlar, oluşturma tarihi, belirtilen iptal tarihten önce iptal edilir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Neden > öğesi hiçbir zaman sistem tarafından okunur. Bu kullanıcı tarafından okunabilen bir iptal nedeni depolamak için yalnızca kullanışlı bir yerdir.
