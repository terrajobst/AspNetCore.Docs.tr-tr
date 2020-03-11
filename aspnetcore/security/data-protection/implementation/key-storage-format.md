---
title: ASP.NET Core 'de anahtar depolama biçimi
author: rick-anderson
description: ASP.NET Core veri koruma anahtarı depolama biçiminin uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667757"
---
# <a name="key-storage-format-in-aspnet-core"></a>ASP.NET Core 'de anahtar depolama biçimi

<a name="data-protection-implementation-key-storage-format"></a>

Nesneler, XML gösteriminde Rest olarak depolanır. Anahtar depolama için varsayılan dizin%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\. ' dir

## <a name="the-key-element"></a>\<Key > öğesi

Anahtarlar, anahtar deposundaki üst düzey nesneler olarak mevcuttur. Kural anahtarlarına göre anahtar adı **-{Guid}. xml**, burada {GUID} anahtar kimliğidir. Bu tür dosyalar tek bir anahtar içerir. Dosya biçimi aşağıdaki gibidir.

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

\<Key > öğesi aşağıdaki öznitelikleri ve alt öğeleri içerir:

* Anahtar kimliği. Bu değer yetkili olarak değerlendirilir; dosya adı, insan okunabilirlik için yalnızca bir nicsan.

* Şu anda 1 ' de düzeltilen \<Key > öğesinin sürümü.

* Anahtarın oluşturulma, etkinleştirme ve sona erme tarihleri.

* Bu anahtarda yer alan kimliği doğrulanmış şifreleme uygulamasıyla ilgili bilgileri içeren bir \<tanımlayıcısı > öğesi.

Yukarıdaki örnekte, anahtarın kimliği {80732141-EC8F-4b80-af9c-c4d2d1ff8901}, 19 Mart 2015 ' de oluşturulmuştur ve etkinleştirilir ve bu süre için gün 90 ömrü vardır. (Bazen etkinleştirme tarihi, bu örnekte olduğu gibi, oluşturma tarihinden biraz önce olabilir. Bunun nedeni, API 'Lerin çalıştığı ve çok zararsız olan bir NBT yöntemidir.)

## <a name="the-descriptor-element"></a>\<Descriptor > öğesi

Dış \<tanımlayıcısı > öğesi, ıauthenticatedencryptordescriptordeserializer uygulayan bir türün derleme nitelikli adı olan bir deserializerType özniteliği içerir. Bu tür, iç \<tanımlayıcı > öğesini okumaktan ve içinde yer alan bilgilerin ayrıştırılmasından sorumludur.

\<Descriptor > öğesinin belirli biçimi, anahtarla kapsüllenmiş kimliği doğrulanmış Şifreleyici uygulamasına bağlıdır ve her bir seri hale getirici türü bunun için biraz farklı bir biçim bekler. Genellikle, bu öğe algoritmik bilgilerini (adlar, türler, OID 'ler veya benzer) ve gizli anahtar malzemesini içerir. Yukarıdaki örnekte, tanımlayıcı bu anahtarın AES-256-CBC şifreleme + HMACSHA256 doğrulamasını kaydırılacağını belirtir.

## <a name="the-encryptedsecret-element"></a>\<encryptedSecret > öğesi

Gizli anahtar malzemesinin şifreli biçimini içeren **&lt;encryptedsecret&gt;** öğesi [, bekleyen gizli dizi şifrelemesi etkinse](xref:security/data-protection/implementation/key-encryption-at-rest)bulunabilir. Öznitelik `decryptorType`, [ıxmldecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor)uygulayan bir türün derleme nitelikli adıdır. Bu tür, Inner **&lt;encryptedKey&gt;** öğesini okumaktan ve özgün düz metin kurtarmak için şifresini çözmekten sorumludur.

`<descriptor>`olduğu gibi, `<encryptedSecret>` öğesinin belirli biçimi kullanımda olan Rest şifreleme mekanizmasına bağlıdır. Yukarıdaki örnekte, ana anahtar açıklama başına Windows DPAPI kullanılarak şifrelenir.

## <a name="the-revocation-element"></a>\<iptal > öğesi

İptal edilecek öğeler, anahtar deposundaki en üst düzey nesneler olarak mevcuttur. Kural iptalinde, dosya adı **iptali-{timestamp}. xml** (belirli bir tarihten önceki tüm anahtarları iptal etmek için) veya **iptal-{GUID}. xml** (belirli bir anahtarı iptal etmek için). Her dosya tek bir \<iptal > öğesi içerir.

Tek tek anahtarların iptal edilmesi için dosya içerikleri aşağıdaki gibi olacaktır.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

Bu durumda, yalnızca belirtilen anahtar iptal edilir. Anahtar kimliği "*" ise, ancak aşağıdaki örnekte olduğu gibi, oluşturma tarihi belirtilen iptal tarihi 'nden önce olan tüm anahtarlar iptal edilir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<neden > öğesi sistem tarafından hiçbir şekilde okunamaz. Yalnızca bir iptal için okunabilir bir neden depolamak için kullanışlı bir yerdir.
