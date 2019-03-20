---
title: ASP.NET core'da anahtar depolama biçimi
author: rick-anderson
description: ASP.NET Core veri koruma anahtar depolama biçimi uygulama ayrıntılarını öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208024"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="255be-103">ASP.NET core'da anahtar depolama biçimi</span><span class="sxs-lookup"><span data-stu-id="255be-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="255be-104">Nesneleri, bekleyen veri olarak XML gösterimi depolanır.</span><span class="sxs-lookup"><span data-stu-id="255be-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="255be-105">Anahtar depolama alanı için varsayılan % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\ dizindir.</span><span class="sxs-lookup"><span data-stu-id="255be-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="255be-106">\<Anahtarı > öğesi</span><span class="sxs-lookup"><span data-stu-id="255be-106">The \<key> element</span></span>

<span data-ttu-id="255be-107">Anahtar deposu en üst düzey nesneleri olarak anahtarları mevcut.</span><span class="sxs-lookup"><span data-stu-id="255be-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="255be-108">Kural gereği, dosya adı anahtarlara sahip **anahtarı-{GUID} .xml**, {GUID} anahtarının kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="255be-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="255be-109">Her bir dosya, tek bir anahtar içeriyor.</span><span class="sxs-lookup"><span data-stu-id="255be-109">Each such file contains a single key.</span></span> <span data-ttu-id="255be-110">Dosya biçimi aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="255be-110">The format of the file is as follows.</span></span>

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

<span data-ttu-id="255be-111">\<Anahtarı > öğesi, aşağıdaki öznitelikler ve alt öğeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="255be-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="255be-112">Anahtar kimliği. Bu değer, yetkili olarak kabul edilir; yalnızca bir nicety okunabilirlik için dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="255be-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="255be-113">Sürümü \<anahtarı > öğesinde, şu anda 1 sabit.</span><span class="sxs-lookup"><span data-stu-id="255be-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="255be-114">Anahtarın oluşturulması, etkinleştirme ve sona erme tarihleri.</span><span class="sxs-lookup"><span data-stu-id="255be-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="255be-115">A \<tanımlayıcısı > Bu anahtarı içinde yer alan kimliği doğrulanmış şifreleme uygulama hakkında bilgi içeren öğe.</span><span class="sxs-lookup"><span data-stu-id="255be-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="255be-116">Yukarıdaki örnekte, {80732141-ec8f-4b80-af9c-c4d2d1ff8901} anahtarının kimliğidir, onu oluşturuldu ve 19 Mart 2015'te etkinleştirilen ve 90 gün ömrü vardır.</span><span class="sxs-lookup"><span data-stu-id="255be-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="255be-117">(Bazen etkinleştirme tarihi biraz önce bu örnekte olduğu gibi oluşturma tarihi olabilir.</span><span class="sxs-lookup"><span data-stu-id="255be-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="255be-118">Bu API'leri nasıl çalışır ve uygulamada zararsızdır bir nEtki kaynaklanır.)</span><span class="sxs-lookup"><span data-stu-id="255be-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="255be-119">\<Tanımlayıcısı > öğesi</span><span class="sxs-lookup"><span data-stu-id="255be-119">The \<descriptor> element</span></span>

<span data-ttu-id="255be-120">Dış \<tanımlayıcısı > öğesi IAuthenticatedEncryptorDescriptorDeserializer uygulayan bir tür bütünleştirilmiş kodla nitelenen adı olan bir öznitelik deserializerType içerir.</span><span class="sxs-lookup"><span data-stu-id="255be-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="255be-121">Bu tür, iç okumak için sorumlu \<tanımlayıcısı > öğesi ve içinde yer alan bilgileri ayrıştırılıyor.</span><span class="sxs-lookup"><span data-stu-id="255be-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="255be-122">Belirli biçimi \<tanımlayıcısı > öğesi kapsüllenmiş anahtarı tarafından kimliği doğrulanmış bir Şifreleyici uygulama bağlıdır ve her bir seri durumdan çıkarıcının türü biraz farklı bir biçim bunun için bekliyor.</span><span class="sxs-lookup"><span data-stu-id="255be-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="255be-123">Genel olarak, bu öğe algoritmik bilgilerini yine de içerecektir (adları, türleri, OID veya benzer) ve gizli anahtar malzemesi.</span><span class="sxs-lookup"><span data-stu-id="255be-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="255be-124">Yukarıdaki örnekte, bu anahtarı AES 256 CBC şifreleme + HMACSHA256 doğrulama sarmalar tanımlayıcısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="255be-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="255be-125">\<EncryptedSecret > öğesi</span><span class="sxs-lookup"><span data-stu-id="255be-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="255be-126">Bir **&lt;encryptedSecret&gt;** şifrelenmiş gizli anahtar malzemesi içeren öğe mevcut olması durumunda [gizli anahtarlarının, bekleyen veri şifrelemesi etkin](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="255be-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="255be-127">Öznitelik `decryptorType` uygulayan bir tür bütünleştirilmiş kodla nitelenen adı [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="255be-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="255be-128">Bu tür, iç okumak için sorumlu **&lt;encryptedKey&gt;** öğesi ve özgün düz metin kurtarmak için şifre çözme.</span><span class="sxs-lookup"><span data-stu-id="255be-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="255be-129">Olduğu gibi `<descriptor>`, belirli biçimi `<encryptedSecret>` öğe kullanımda bekleyen şifreleme mekanizması bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="255be-129">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="255be-130">Yukarıdaki örnekte, ana anahtarı açıklama Windows DPAPI kullanılarak şifrelenir.</span><span class="sxs-lookup"><span data-stu-id="255be-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="255be-131">\<İptal > öğesi</span><span class="sxs-lookup"><span data-stu-id="255be-131">The \<revocation> element</span></span>

<span data-ttu-id="255be-132">Geri alma işlemleri, anahtar deposu en üst düzey nesneleri olarak mevcut.</span><span class="sxs-lookup"><span data-stu-id="255be-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="255be-133">Kural gereği, dosya adını geri alma işlemleri sahip **iptal-{zaman damgası} .xml** (için belirli bir tarihten önce tüm anahtarlar iptal) veya **iptal-{GUID} .xml** (için belirli bir anahtarı iptal etme).</span><span class="sxs-lookup"><span data-stu-id="255be-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="255be-134">Her dosyada tek bir \<iptal > öğesi.</span><span class="sxs-lookup"><span data-stu-id="255be-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="255be-135">Dosya içeriği için geri alma işlemleri ayrı ayrı anahtarların olacak şekilde aşağıda.</span><span class="sxs-lookup"><span data-stu-id="255be-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="255be-136">Bu durumda, yalnızca belirtilen anahtarı iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="255be-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="255be-137">Anahtar kimliği ise "\*", ancak olarak aşağıdaki örnekte, tüm anahtarlar, oluşturma tarihi, belirtilen iptal tarihten önce iptal edilir.</span><span class="sxs-lookup"><span data-stu-id="255be-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="255be-138">\<Neden > öğesi hiçbir zaman sistem tarafından okunur.</span><span class="sxs-lookup"><span data-stu-id="255be-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="255be-139">Bu kullanıcı tarafından okunabilen bir iptal nedeni depolamak için yalnızca kullanışlı bir yerdir.</span><span class="sxs-lookup"><span data-stu-id="255be-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
