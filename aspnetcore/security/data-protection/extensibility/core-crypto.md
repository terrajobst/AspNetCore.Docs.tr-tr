---
title: ASP.NET Core çekirdek şifreleme genişletilebilirliği
author: rick-anderson
description: IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer ve en üst düzey Fabrika hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.date: 8/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b5a0dbc9120a8032dbb8d8eee74684495a982ac1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896832"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>ASP.NET Core çekirdek şifreleme genişletilebilirliği

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** şifreleme alt sistemi temel yapı bloğu bir arabirimdir. Genellikle bir IAuthenticatedEncryptor anahtar başına yoktur ve tüm şifreleme anahtar malzemesi ve şifreleme işlemlerini gerçekleştirmek için gerekli olan algoritmik bilgileri IAuthenticatedEncryptor örneği sarmalar.

Adından da anlaşılacağı gibi türü kimliği doğrulanmış şifreleme ve şifre çözme hizmetleri sağlamaktan sorumludur. Aşağıdaki iki API'leri gösterir.

* Şifre çözme (ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData): byte]

* Şifreleme (ArraySegment<byte> düz metin, ArraySegment<byte> additionalAuthenticatedData): byte]

Şifreleme yöntemi enciphered düz metin ve bir kimlik doğrulama etiketi içeren blob döndürür. AAD son yükü kurtarılabilir olması gerekmez ancak kimlik doğrulaması etiketi ek kimliği doğrulanmış veriler (AAD) kapsamalıdır. Şifre çözme yöntemi kimlik doğrulaması etiketi doğrular ve deciphered yükü döndürür. Tüm hataları (ArgumentNullException dışında ve benzer) için CryptographicException homogenized.

> [!NOTE]
> IAuthenticatedEncryptor örneği gerçekte anahtar malzemesi içermesi gerekmez. Örneğin, uygulama için tüm işlemleri için bir HSM atayabilirsiniz.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Bir IAuthenticatedEncryptor oluşturma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** arabirimi oluşturmak bildiği bir türü temsil eden bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği. Kendi API aşağıdaki gibidir.

* CreateEncryptorInstance (IKey anahtarı): IAuthenticatedEncryptor

Verilen tüm IKey örneği için kendi CreateEncryptorInstance yöntemi tarafından oluşturulan tüm kimliği doğrulanmış encryptors eşdeğer olarak düşünülmesi gereken örnek kod aşağıda.

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor** arabirimi oluşturmak bildiği bir türü temsil eden bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği. Kendi API aşağıdaki gibidir.

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

IAuthenticatedEncryptor gibi IAuthenticatedEncryptorDescriptor örneği belirli bir anahtarı sarmalama varsayılır. Verilen tüm IAuthenticatedEncryptorDescriptor örneği için kendi CreateEncryptorInstance yöntemi tarafından oluşturulan tüm kimliği doğrulanmış encryptors eşdeğer olarak düşünülmesi gereken, yani örnek kod aşağıda.

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET 2.x yalnızca çekirdek)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor** arabirimi kendisini XML biçimine dışa bildiği bir türü temsil eder. Kendi API aşağıdaki gibidir.

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML seri hale getirme

IAuthenticatedEncryptor IAuthenticatedEncryptorDescriptor arasındaki birincil fark, tanımlayıcı Şifreleyici oluşturma ve geçerli bağımsız değişkenlerle tedarik bilir ' dir. Uygulama SymmetricAlgorithm ve KeyedHashAlgorithm dayanır IAuthenticatedEncryptor göz önünde bulundurun. Bu tür tüketmeye Şifreleyici'nın iş, ancak uygun bir uygulamayı yeniden başlatılırsa kendisini yeniden oluşturmak nasıl açıklama gerçekten yazılamıyor şekilde bu mutlaka, bu tür nereden geldiğini bilmiyor. Bu en üstünde daha yüksek bir düzeye tanımlayıcısı görür. Şifreleyici örneğinin nasıl oluşturulacağını tanımlayıcısı bilir bu yana (örneğin, bunu gerekli algoritmaları oluşturma bilen), böylece uygulamanın sıfırladıktan sonra Şifreleyici örneği yeniden bu bilgi XML formundaki serileştirebilen.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Tanımlayıcı kendi ExportToXml yordamı seri hale getirilebilir. Bu yordam iki özellikler içeren bir XmlSerializedDescriptorInfo döndürür: tanımlayıcısı ve temsil eden tür XElement temsili bir [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) olabilen karşılık gelen XElement verilen bu tanımlayıcısı resurrect için kullanılır.

Serileştirilmiş tanımlayıcısı şifreleme anahtar malzemesi gibi hassas bilgiler içerebilir. Veri koruma sisteminde depolama alanına kalıcı önce bilgilerini şifrelemek için yerleşik desteğe sahiptir. Bu yararlanmak için tanımlayıcı öznitelik adı "boşluğu requiresEncryption" ile hassas bilgiler içeren öğe işaretlemeniz gerekir (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), değer "true".

>[!TIP]
> Bu öznitelik ayarlamak için bir yardımcı API yoktur. XElement.MarkAsRequiresEncryption() Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel ad alanında bulunan uzantısı yöntemini çağırın.

Durumları serileştirilmiş tanımlayıcısı hassas bilgileri nerede içermiyor olabilir. Yeniden bir HSM'de depolanan bir şifreleme anahtarını durumunu göz önünde bulundurun. Tanımlayıcı anahtar malzemesi HSM malzeme düz metin biçiminde kullanıma olmaz bu yana kendisine serileştirilirken yazamıyor. Bunun yerine, tanımlayıcı (HSM bu şekilde verme izin veriyorsa) anahtar veya anahtar HSM'ın kendi benzersiz tanımlayıcısı anahtar Sarmalanan sürümünü kullanıma yazabilirsiniz.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** arabirimi bir XElement IAuthenticatedEncryptorDescriptor örneğinden seri durumdan çıkarılacak bildiği bir türü temsil eder. Tek bir yöntem sunar:

* ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor

ImportFromXml yöntemi tarafından döndürülen XElement alır [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) ve özgün IAuthenticatedEncryptorDescriptor bir eşdeğerini oluşturur.

IAuthenticatedEncryptorDescriptorDeserializer uygulayan türleri aşağıdaki iki ortak oluşturuculardan birine sahip olmalıdır:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> Oluşturucuya geçirilen IServiceProvider null olabilir.

## <a name="the-top-level-factory"></a>Üst düzey Fabrika

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** sınıfı temsil eder, nasıl oluşturulacağını bildiği bir tür [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) örnekleri. Tek bir API sunar.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

Üst düzey Fabrika olarak AlgorithmConfiguration düşünün. Yapılandırma şablon olarak görev yapar. Algoritmik bilgi sarmalar (örn., bu yapılandırma bir AES 128 GCM ana anahtar ile tanımlayıcıları üretir), ancak henüz belirli bir anahtarı ile ilişkilendirilmiş.

CreateNewDescriptor olarak adlandırılır, yalnızca bu çağrı için yeni anahtar malzemesi oluşturulduğunda ve yeni IAuthenticatedEncryptorDescriptor üretilir, bu anahtar malzeme ve malzeme kullanmak için gereken algoritmik bilgileri sarmalar. Anahtar malzemesi yazılımda oluşturulabilir (ve bellekte tutulan), onu oluşturulabilir ve bir HSM ve benzeri içinde tutulur. CreateNewDescriptor herhangi iki çağrıları eşdeğer IAuthenticatedEncryptorDescriptor örnekleri hiçbir zaman oluşturmalısınız önemli noktasıdır.

Gibi AlgorithmConfiguration türü anahtar oluşturma yordamları için giriş noktası olarak hizmet [çalışırken otomatik anahtar](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Tüm Gelecekteki anahtarların uygulamasını değiştirmek için KeyManagementOptions AuthenticatedEncryptorConfiguration özelliğini ayarlayın.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** arabirimi temsil eder, nasıl oluşturulacağını bildiği bir tür [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) örnekleri. Tek bir API sunar.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

Üst düzey Fabrika olarak IAuthenticatedEncryptorConfiguration düşünün. Yapılandırma şablon olarak görev yapar. Algoritmik bilgi sarmalar (örn., bu yapılandırma bir AES 128 GCM ana anahtar ile tanımlayıcıları üretir), ancak henüz belirli bir anahtarı ile ilişkilendirilmiş.

CreateNewDescriptor olarak adlandırılır, yalnızca bu çağrı için yeni anahtar malzemesi oluşturulduğunda ve yeni IAuthenticatedEncryptorDescriptor üretilir, bu anahtar malzeme ve malzeme kullanmak için gereken algoritmik bilgileri sarmalar. Anahtar malzemesi yazılımda oluşturulabilir (ve bellekte tutulan), onu oluşturulabilir ve bir HSM ve benzeri içinde tutulur. CreateNewDescriptor herhangi iki çağrıları eşdeğer IAuthenticatedEncryptorDescriptor örnekleri hiçbir zaman oluşturmalısınız önemli noktasıdır.

Gibi IAuthenticatedEncryptorConfiguration türü anahtar oluşturma yordamları için giriş noktası olarak hizmet [çalışırken otomatik anahtar](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Tüm Gelecekteki anahtarların uygulamasını değiştirmek için hizmet kapsayıcısında IAuthenticatedEncryptorConfiguration tek kaydedin.

---
