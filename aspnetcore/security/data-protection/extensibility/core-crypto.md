---
title: ASP.NET core'da çekirdek şifreleme genişletilebilirliği
author: rick-anderson
description: IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer ve üst düzey factory hakkında bilgi edinin.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: cf4a142992efe5b00a75285ef9ad9735fe7be411
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209076"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>ASP.NET core'da çekirdek şifreleme genişletilebilirliği

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** şifreleme alt sistemi temel yapı bloğu arabirimidir. Genellikle anahtar başına bir IAuthenticatedEncryptor yoktur ve tüm şifreleme anahtar malzemesi ve şifreleme işlemleri gerçekleştirmesi için gerekli olan algoritmik bilgileri IAuthenticatedEncryptor örneğine sarar.

Adından da anlaşılacağı gibi tür kimliği doğrulanmış şifreleme ve şifre çözme hizmetleri sağlamaktan sorumludur. Bunu, aşağıdaki iki API'lerini kullanıma sunar.

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

Şifreleme yöntemi enciphered düz metin ve kimlik doğrulaması etiketi içeren bir blob döndürür. AAD son yükten kurtarılabilir olması gerekmez ancak ek kimliği doğrulanmış veriler (AAD) kimlik doğrulaması etiketi kapsamalıdır. Şifre çözme yöntemi, kimlik doğrulaması etiketi doğrular ve deciphered yükü döndürür. Tüm hataları (ArgumentNullException dışında ve benzer) için CryptographicException homogenized.

> [!NOTE]
> IAuthenticatedEncryptor örneği gerçekten anahtar malzemesi içermesi gerekmez. Örneğin, uygulama tüm işlemler için HSM temsilci seçebilecek.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Bir IAuthenticatedEncryptor oluşturma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** arabirimi oluşturmak bildiği bir türü temsil eder bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği. Kendi API aşağıdaki gibidir.

* CreateEncryptorInstance (IKey anahtarı): IAuthenticatedEncryptor

Verilen tüm IKey örneği için kendi CreateEncryptorInstance yöntemi tarafından oluşturulan tüm kimliği doğrulanmış encryptors eşdeğer olarak düşünülmesi gereken aşağıdaki kod örneği.

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

**IAuthenticatedEncryptorDescriptor** arabirimi oluşturmak bildiği bir türü temsil eder bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği. Kendi API aşağıdaki gibidir.

* CreateEncryptorInstance() : IAuthenticatedEncryptor

* ExportToXml() : XmlSerializedDescriptorInfo

IAuthenticatedEncryptor gibi belirli bir anahtarı sarmalama IAuthenticatedEncryptorDescriptor örneğini varsayılır. Bu tüm verilen IAuthenticatedEncryptorDescriptor örneği için kendi CreateEncryptorInstance yöntemi tarafından oluşturulan tüm kimliği doğrulanmış encryptors eşdeğer olarak değerlendirilmesi gerektiğini anlamına gelir. aşağıdaki kod örneği.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x yalnızca)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor** arabirimi kendisini XML biçimine dışa bildiği bir türü temsil eder. Kendi API aşağıdaki gibidir.

* ExportToXml() : XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>XML seri hale getirme

IAuthenticatedEncryptor IAuthenticatedEncryptorDescriptor arasındaki birincil fark tanımlayıcı Şifreleyici oluşturma ve geçerli bağımsız değişkenlerle tedarik bilir. Uygulaması SymmetricAlgorithm ve KeyedHashAlgorithm kullanır bir IAuthenticatedEncryptor göz önünde bulundurun. Bu tür tüketmeye Şifreleyici'nın iş, ancak uygulama yeniden başlatılırsa kendisi yeniden oluşturmak nasıl uygun açıklamasını gerçekten yazamaz şekilde, mutlaka, bu tür bir nereden geldiğini bilmez. Tanımlayıcı, bu üzerinde daha yüksek bir düzeye olarak görev yapar. Tanımlayıcı Şifreleyici örneği oluşturmak nasıl bilir olduğundan (örneğin, gerekli algoritmalar oluşturma Giden), böylece uygulama sıfırladıktan sonra Şifreleyici örneği yeniden oluşturulabilir, Bilgi Bankası XML formundaki serileştirebiliyorsa.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Tanımlayıcı, ExportToXml yordamı seri hale getirilebilir. Bu yordam, iki özellik içeren bir XmlSerializedDescriptorInfo döndürür: tanımlayıcı ve temsil eden tür XElement temsili bir [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) olabilir. karşılık gelen XElement verilen bu tanımlayıcıyı resurrect için kullanılır.

Seri hale getirilmiş tanımlayıcı şifreleme anahtar malzemesi gibi hassas bilgiler içerebilir. Veri koruma sisteminde kalıcı depolama için önce bilgi şifreleme için yerleşik destek sunmaktadır. Bu yararlanmak için tanımlayıcı öznitelik adı "boşluğu requiresEncryption" ile hassas bilgiler içeren öğe işaretlemeniz gerekir (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), değeri "true".

>[!TIP]
> Bu öznitelik ayarlamak için bir yardımcı API yoktur. XElement.MarkAsRequiresEncryption() Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel ad alanında bulunan uzantı yöntemini çağırın.

Servis taleplerini seri hale getirilmiş tanımlayıcı hassas bilgileri burada içermiyor olabilir. Tekrar bir şifreleme anahtarının bir HSM'de depolanan bir durum düşünün. Tanımlayıcı, HSM malzeme düz metin biçiminde göstermek olmaz bu yana kendisine serileştirilirken anahtar malzemesi yazılamıyor. Bunun yerine, tanımlayıcı (HSM bu biçimde dışa aktarma izin verirse), anahtar veya anahtar HSM'ın kendi benzersiz tanımlayıcısını anahtar sarmalanmış sürümünü kullanıma yazabilirsiniz.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** arabirimi bir XElement IAuthenticatedEncryptorDescriptor örneğinden seri durumdan çıkarılacak bildiği bir türü temsil eder. Bu, tek bir yöntemi gösterir:

* ImportFromXml (XElement öğesi): IAuthenticatedEncryptorDescriptor

ImportFromXml yöntem tarafından döndürülen XElement alır [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) ve özgün IAuthenticatedEncryptorDescriptor eşdeğer oluşturur.

IAuthenticatedEncryptorDescriptorDeserializer uygulayan türleri, aşağıdaki iki genel oluşturucular biri olmalıdır:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> Oluşturucuya geçirilen IServiceProvider null olabilir.

## <a name="the-top-level-factory"></a>Üst düzey Fabrika

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** sınıfı oluşturmak bildiği bir türü temsil eder [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) örnekleri. Bu, tek bir API sunar.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Üst düzey Fabrika olarak AlgorithmConfiguration düşünün. Yapılandırma şablon olarak görev yapar. Algoritmik bilgi sarmalar (örneğin, bu yapılandırma bir AES-128-GCM ana anahtar ile tanımlayıcıları üretir), ancak henüz belirli bir anahtar ile ilişkili.

CreateNewDescriptor çağrılır, yeni anahtar malzemesi bu çağrı için yalnızca oluşturulduğunda ve yeni IAuthenticatedEncryptorDescriptor üretilir, bu anahtar malzemesi ve malzeme kullanmak için gereken bilgilerin algoritmik sarmalar. Anahtar malzemesi yazılımda oluşturulabilir (ve bellekte tutulan), bunu oluşturulabilir ve bir HSM ve benzeri tutulan. İki çağrıları CreateNewDescriptor eşdeğer IAuthenticatedEncryptorDescriptor örnekleri hiçbir zaman oluşturmalısınız önemli noktasıdır.

Gibi AlgorithmConfiguration türü anahtarı oluşturma rutinleri için giriş noktası olarak hizmet [çalışırken otomatik anahtar](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Gelecekteki tüm anahtarların uygulamasını değiştirmek için KeyManagementOptions AuthenticatedEncryptorConfiguration özelliğini ayarlayın.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** arabirimi oluşturmak bildiği bir türü temsil eder [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) örnekleri. Bu, tek bir API sunar.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Üst düzey Fabrika olarak IAuthenticatedEncryptorConfiguration düşünün. Yapılandırma şablon olarak görev yapar. Algoritmik bilgi sarmalar (örneğin, bu yapılandırma bir AES-128-GCM ana anahtar ile tanımlayıcıları üretir), ancak henüz belirli bir anahtar ile ilişkili.

CreateNewDescriptor çağrılır, yeni anahtar malzemesi bu çağrı için yalnızca oluşturulduğunda ve yeni IAuthenticatedEncryptorDescriptor üretilir, bu anahtar malzemesi ve malzeme kullanmak için gereken bilgilerin algoritmik sarmalar. Anahtar malzemesi yazılımda oluşturulabilir (ve bellekte tutulan), bunu oluşturulabilir ve bir HSM ve benzeri tutulan. İki çağrıları CreateNewDescriptor eşdeğer IAuthenticatedEncryptorDescriptor örnekleri hiçbir zaman oluşturmalısınız önemli noktasıdır.

Gibi IAuthenticatedEncryptorConfiguration türü anahtarı oluşturma rutinleri için giriş noktası olarak hizmet [çalışırken otomatik anahtar](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Gelecekteki tüm anahtarların uygulamasını değiştirmek için hizmet kapsayıcısında IAuthenticatedEncryptorConfiguration tek kaydedin.

---
