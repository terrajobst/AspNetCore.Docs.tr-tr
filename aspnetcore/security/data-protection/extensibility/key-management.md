---
title: ASP.NET Core anahtar yönetimi genişletilebilirliği
author: rick-anderson
description: ASP.NET Core veri koruma anahtar yönetimi genişletilebilirliği hakkında bilgi edinin.
ms.author: riande
ms.date: 11/22/2017
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 965a7ed8ca2f72a66cfe093b5978a54fea5440fd
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219322"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="45a14-103">ASP.NET Core anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="45a14-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="45a14-104">Okuma [anahtar yönetimi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) bölümden önce bu bölümü okunurken gibi bazı bu API'leri temel kavramları açıklar.</span><span class="sxs-lookup"><span data-stu-id="45a14-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="45a14-105">Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.</span><span class="sxs-lookup"><span data-stu-id="45a14-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="45a14-106">Anahtar</span><span class="sxs-lookup"><span data-stu-id="45a14-106">Key</span></span>

<span data-ttu-id="45a14-107">`IKey` Cryptosystem anahtarında temel gösterimini arabirimidir.</span><span class="sxs-lookup"><span data-stu-id="45a14-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="45a14-108">Terim anahtarı burada "şifreleme anahtar malzemesi" değişmez değer duygusu değil, soyut anlamında kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="45a14-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="45a14-109">Bir anahtarı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="45a14-109">A key has the following properties:</span></span>

* <span data-ttu-id="45a14-110">Etkinleştirme, oluşturma ve sona erme tarihleri</span><span class="sxs-lookup"><span data-stu-id="45a14-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="45a14-111">İptal durumu</span><span class="sxs-lookup"><span data-stu-id="45a14-111">Revocation status</span></span>

* <span data-ttu-id="45a14-112">Anahtar tanımlayıcısını (GUID)</span><span class="sxs-lookup"><span data-stu-id="45a14-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="45a14-113">Ayrıca, `IKey` sunan bir `CreateEncryptor` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.</span><span class="sxs-lookup"><span data-stu-id="45a14-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="45a14-114">Ayrıca, `IKey` sunan bir `CreateEncryptorInstance` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.</span><span class="sxs-lookup"><span data-stu-id="45a14-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="45a14-115">Ham şifreleme malzemesini almak için hiçbir API yoktur bir `IKey` örneği.</span><span class="sxs-lookup"><span data-stu-id="45a14-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="45a14-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="45a14-116">IKeyManager</span></span>

<span data-ttu-id="45a14-117">`IKeyManager` Arabirimi genel anahtar depolama alanı, alma ve düzenleme için sorumlu bir nesneyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="45a14-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="45a14-118">Bu, üç üst düzey işlemini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="45a14-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="45a14-119">Yeni bir anahtar oluşturun ve depolama için kalıcı.</span><span class="sxs-lookup"><span data-stu-id="45a14-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="45a14-120">Tüm anahtarları, depolama alanından alma.</span><span class="sxs-lookup"><span data-stu-id="45a14-120">Get all keys from storage.</span></span>

* <span data-ttu-id="45a14-121">Bir veya daha fazla anahtarı iptal edin ve depolama için iptal bilgilerini kalıcı hale.</span><span class="sxs-lookup"><span data-stu-id="45a14-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="45a14-122">Yazma bir `IKeyManager` çok gelişmiş bir görevdir ve geliştiricilerin çoğu, çalışmayın.</span><span class="sxs-lookup"><span data-stu-id="45a14-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="45a14-123">Bunun yerine, çoğu geliştirici tarafından sunulan özellikleri yararlanmak [XmlKeyManager](#xmlkeymanager) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="45a14-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="45a14-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="45a14-124">XmlKeyManager</span></span>

<span data-ttu-id="45a14-125">`XmlKeyManager` Türüdür yerleşik somut uygulamasını `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="45a14-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="45a14-126">Bu anahtar emanet ve anahtarları, bekleyen veri şifrelemesi gibi birçok yararlı olanakları sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a14-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="45a14-127">Bu sistemde anahtarları XML öğeleri olarak temsil edilir (özellikle [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="45a14-127">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="45a14-128">`XmlKeyManager` görevleri yerine getirerek sırasında birkaç diğer bileşenlere bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="45a14-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="45a14-129">`AlgorithmConfiguration`, yeni anahtarlar tarafından kullanılan algoritmalar belirler.</span><span class="sxs-lookup"><span data-stu-id="45a14-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="45a14-130">`IXmlRepository`, burada anahtarları kalıcı depolama alanında hangi denetimleri.</span><span class="sxs-lookup"><span data-stu-id="45a14-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="45a14-131">`IXmlEncryptor` [isteğe bağlı], anahtarları, bekleyen veri şifreleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a14-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="45a14-132">`IKeyEscrowSink` [isteğe bağlı], anahtar emanet hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a14-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="45a14-133">`IXmlRepository`, burada anahtarları kalıcı depolama alanında hangi denetimleri.</span><span class="sxs-lookup"><span data-stu-id="45a14-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="45a14-134">`IXmlEncryptor` [isteğe bağlı], anahtarları, bekleyen veri şifreleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a14-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="45a14-135">`IKeyEscrowSink` [isteğe bağlı], anahtar emanet hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="45a14-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="45a14-136">Nasıl bu bileşenler birlikte içinde kablolu arabirimlerdir gösteren yüksek düzey şemalarını aşağıda verilmiştir `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="45a14-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation2.png)

<span data-ttu-id="45a14-138">*Anahtar oluşturma / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="45a14-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="45a14-139">Uygulamasında `CreateNewKey`, `AlgorithmConfiguration` benzersiz bir oluşturmak için kullanılan bileşen `IAuthenticatedEncryptorDescriptor`, hangi sonra serileştirilmiş XML olarak.</span><span class="sxs-lookup"><span data-stu-id="45a14-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="45a14-140">Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="45a14-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="45a14-141">Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş bir XML belgesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="45a14-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="45a14-142">Şifrelenmiş bu belgenin uzun vadeli depolama için kalıcı `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="45a14-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="45a14-143">(Hiçbir `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge içinde kalıcı `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="45a14-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Anahtar alma](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation1.png)

<span data-ttu-id="45a14-146">*Anahtar oluşturma / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="45a14-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="45a14-147">Uygulamasında `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` benzersiz bir oluşturmak için kullanılan bileşen `IAuthenticatedEncryptorDescriptor`, hangi sonra serileştirilmiş XML olarak.</span><span class="sxs-lookup"><span data-stu-id="45a14-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="45a14-148">Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="45a14-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="45a14-149">Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş bir XML belgesi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="45a14-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="45a14-150">Şifrelenmiş bu belgenin uzun vadeli depolama için kalıcı `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="45a14-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="45a14-151">(Hiçbir `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge içinde kalıcı `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="45a14-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Anahtar alma](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="45a14-153">*Anahtar alma / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="45a14-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="45a14-154">Uygulamasında `GetAllKeys`, XML belgeleri temsil eden tuşları ve geri alma işlemleri temel okunur `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="45a14-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="45a14-155">Bu belgeler şifrelenir, sistem otomatik olarak bunların şifresini çözer.</span><span class="sxs-lookup"><span data-stu-id="45a14-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="45a14-156">`XmlKeyManager` uygun oluşturur `IAuthenticatedEncryptorDescriptorDeserializer` belgeleri seri durumdan çıkarılacak örnekleri yeniden `IAuthenticatedEncryptorDescriptor` örnekleri, sonra ayrı ayrı sarmalanır `IKey` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="45a14-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="45a14-157">Bu koleksiyonu `IKey` örnekleri, çağırana döndürülür.</span><span class="sxs-lookup"><span data-stu-id="45a14-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="45a14-158">Belirli bir XML öğeleri hakkında daha fazla bilgi bulunabilir [anahtar depolama biçimi belge](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="45a14-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="45a14-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="45a14-159">IXmlRepository</span></span>

<span data-ttu-id="45a14-160">`IXmlRepository` Arabirimi XML kalıcı hale getirmek ve yedekleme deposundan XML almak bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="45a14-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="45a14-161">Bu, iki API'lerini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="45a14-161">It exposes two APIs:</span></span>

* <span data-ttu-id="45a14-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="45a14-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="45a14-163">Uygulamaları `IXmlRepository` aracılığıyla geçirme XML'i ayrıştırmakta gerekmez.</span><span class="sxs-lookup"><span data-stu-id="45a14-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="45a14-164">XML belgeleri donuk ele almanız ve belgelerini ayrıştırma ve oluşturma hakkında endişe daha yüksek katmanları olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="45a14-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="45a14-165">Uygulayan dört yerleşik somut tür `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="45a14-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

* [<span data-ttu-id="45a14-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="45a14-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="45a14-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="45a14-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="45a14-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="45a14-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="45a14-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="45a14-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

<span data-ttu-id="45a14-170">Bkz: [anahtar depolama sağlayıcıları belge](xref:security/data-protection/implementation/key-storage-providers) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="45a14-170">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="45a14-171">Özel bir kayıt `IXmlRepository` farklı yedekleme deposu (örneğin, Azure tablo depolama) kullanılırken uygundur.</span><span class="sxs-lookup"><span data-stu-id="45a14-171">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="45a14-172">Varsayılan depo birçok farklı uygulama değiştirmek için özel bir kayıt `IXmlRepository` örneği:</span><span class="sxs-lookup"><span data-stu-id="45a14-172">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="45a14-173">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="45a14-173">IXmlEncryptor</span></span>

<span data-ttu-id="45a14-174">`IXmlEncryptor` Arabirimi düz metin XML öğesi şifreleyebilirsiniz bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="45a14-174">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="45a14-175">Bu, tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="45a14-175">It exposes a single API:</span></span>

* <span data-ttu-id="45a14-176">(XElement plaintextElement) şifrelemek: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="45a14-176">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="45a14-177">Seri hale getirilmiş bir, `IAuthenticatedEncryptorDescriptor` işaretlenmiş ardından "şifreleme gerektiren" tüm öğeleri içerir `XmlKeyManager` bu öğeleri yapılandırılan çalışacak `IXmlEncryptor`'s `Encrypt` yöntemi ve kalıcı hale gelir enciphered öğe yerine düz metin öğesine `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="45a14-177">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="45a14-178">Çıkışı `Encrypt` yöntemi bir `EncryptedXmlInfo` nesne.</span><span class="sxs-lookup"><span data-stu-id="45a14-178">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="45a14-179">Bu nesne enciphered hem sonuç içeren bir sarmalayıcı olan `XElement` ve türünü temsil eden bir `IXmlDecryptor` karşılık gelen öğe çözmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="45a14-179">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="45a14-180">Uygulayan dört yerleşik somut tür `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="45a14-180">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="45a14-181">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="45a14-181">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="45a14-182">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="45a14-182">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="45a14-183">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="45a14-183">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="45a14-184">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="45a14-184">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="45a14-185">Bkz: [rest belge, anahtar şifreleme](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="45a14-185">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="45a14-186">Varsayılan bekleyen anahtar şifreleme mekanizması birçok farklı uygulama değiştirmek için özel bir kayıt `IXmlEncryptor` örneği:</span><span class="sxs-lookup"><span data-stu-id="45a14-186">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="45a14-187">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="45a14-187">IXmlDecryptor</span></span>

<span data-ttu-id="45a14-188">`IXmlDecryptor` Arabirimi şifresini çözmek bildiği bir türü temsil eder bir `XElement` , enciphered aracılığıyla bir `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="45a14-188">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="45a14-189">Bu, tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="45a14-189">It exposes a single API:</span></span>

* <span data-ttu-id="45a14-190">(XElement encryptedElement) şifresini: XElement</span><span class="sxs-lookup"><span data-stu-id="45a14-190">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="45a14-191">`Decrypt` Yöntemi tarafından gerçekleştirilen şifreleme alır `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="45a14-191">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="45a14-192">Genellikle, her somut `IXmlEncryptor` uygulamasına karşılık gelen bir somut sahipseniz `IXmlDecryptor` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="45a14-192">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="45a14-193">Türleri uygulayan `IXmlDecryptor` aşağıdaki iki genel oluşturucular biri olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="45a14-193">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="45a14-194">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="45a14-194">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="45a14-195">.ctor()</span><span class="sxs-lookup"><span data-stu-id="45a14-195">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="45a14-196">`IServiceProvider` Geçirilen oluşturucuya null olabilir.</span><span class="sxs-lookup"><span data-stu-id="45a14-196">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="45a14-197">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="45a14-197">IKeyEscrowSink</span></span>

<span data-ttu-id="45a14-198">`IKeyEscrowSink` Arabirimi hassas bilgilerin emanet gerçekleştirebileceği bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="45a14-198">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="45a14-199">Seri hale getirilmiş tanımlayıcıları (şifreleme malzemelerini gibi) hassas bilgiler içerebilir ve ne için giriş neden budur geri çağırma [IXmlEncryptor](#ixmlencryptor) ilk başta yazın.</span><span class="sxs-lookup"><span data-stu-id="45a14-199">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="45a14-200">Ancak, Kazalar olabilir ve anahtar halkaları silinebilir veya bozulmuş.</span><span class="sxs-lookup"><span data-stu-id="45a14-200">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="45a14-201">Yapılandırılmış tüm tarafından dönüştürülür önce ham serileştirilmiş XML erişimine bir Acil Durum kaçış noktası, emanet arabirimidir [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="45a14-201">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="45a14-202">Arabirim, tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="45a14-202">The interface exposes a single API:</span></span>

* <span data-ttu-id="45a14-203">Store (GUID Keyıd, XElement öğe)</span><span class="sxs-lookup"><span data-stu-id="45a14-203">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="45a14-204">En fazla olan `IKeyEscrowSink` belirtilen öğe işletme ilkesiyle uyumlu güvenli bir şekilde işlemek üzere uygulama.</span><span class="sxs-lookup"><span data-stu-id="45a14-204">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="45a14-205">Sertifikanın özel anahtarı bir yere kalacakları XML öğesi bilinen bir kurumsal X.509 sertifikası kullanarak şifrelemek emanet havuz için bir olası uygulama olabilir; `CertificateXmlEncryptor` türü bu konuda yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="45a14-205">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="45a14-206">`IKeyEscrowSink` Uygulamasıdır ayrıca belirtilen öğe uygun şekilde kalıcı hale getirmekten sorumludur.</span><span class="sxs-lookup"><span data-stu-id="45a14-206">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="45a14-207">Sunucu yöneticileri olabilir ancak varsayılan olarak emanet bir mekanizma, etkin [bu genel yapılandırma](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="45a14-207">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="45a14-208">Ayrıca programlı olarak aracılığıyla yapılandırılabilir `IDataProtectionBuilder.AddKeyEscrowSink` aşağıdaki örnekte gösterildiği gibi yöntemi.</span><span class="sxs-lookup"><span data-stu-id="45a14-208">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="45a14-209">`AddKeyEscrowSink` Yöntemi aşırı yüklemeleri yansıtma `IServiceCollection.AddSingleton` ve `IServiceCollection.AddInstance` aşırı olarak `IKeyEscrowSink` örnekleri teklileri olmasını yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="45a14-209">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="45a14-210">Birden çok `IKeyEscrowSink` örnekleri kayıtlı, her biri anahtar oluşturma sırasında çağrılacak şekilde anahtarları için birden fazla mekanizmayı aynı anda kalacakları.</span><span class="sxs-lookup"><span data-stu-id="45a14-210">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="45a14-211">Malzemesini okumak için hiçbir API yoktur bir `IKeyEscrowSink` örneği.</span><span class="sxs-lookup"><span data-stu-id="45a14-211">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="45a14-212">Emanet mekanizmasının tasarım kuramsal ile tutarlı budur: anahtar malzemesi güvenilir bir yetkili tarafından erişilebilir kılın yönelik ve uygulamanın kendisini güvenilir bir yetkili olmadığından, kendi escrowed malzeme erişimi olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="45a14-212">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="45a14-213">Aşağıdaki örnek kod oluşturma ve kaydetme işlemlerini gösterir bir `IKeyEscrowSink` burada anahtarları kalacakları sağlayacak şekilde yalnızca "CONTOSODomain Admins" üyeleri bunları kurtarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45a14-213">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="45a14-214">Bu örneği çalıştırmak için bir etki alanına katılmış Windows 8'olmalıdır. / Windows Server 2012 makine ve etki alanı denetleyicisi Windows Server 2012 veya üzeri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="45a14-214">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
