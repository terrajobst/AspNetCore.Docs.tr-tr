---
title: ASP.NET Core anahtar yönetimi genişletilebilirliği
author: rick-anderson
description: ASP.NET Core veri koruma anahtar yönetimi genişletilebilirliği hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665881"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="f6a0e-103">ASP.NET Core anahtar yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="f6a0e-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="f6a0e-104">Bu API 'nin arkasındaki temel kavramların bazılarını açıklandığı için, bu bölümü okumadan önce [anahtar yönetimi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) bölümünü okuyun.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="f6a0e-105">Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="f6a0e-106">Anahtar</span><span class="sxs-lookup"><span data-stu-id="f6a0e-106">Key</span></span>

<span data-ttu-id="f6a0e-107">`IKey` arabirimi, Cryptosystem içindeki bir anahtarın temel gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="f6a0e-108">Terim anahtarı burada "şifreleme anahtar malzemesi" değişmez değer duygusu değil, soyut anlamında kullanılmıştır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="f6a0e-109">Bir anahtarı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-109">A key has the following properties:</span></span>

* <span data-ttu-id="f6a0e-110">Etkinleştirme, oluşturma ve sona erme tarihleri</span><span class="sxs-lookup"><span data-stu-id="f6a0e-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="f6a0e-111">İptal durumu</span><span class="sxs-lookup"><span data-stu-id="f6a0e-111">Revocation status</span></span>

* <span data-ttu-id="f6a0e-112">Anahtar tanımlayıcısını (GUID)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f6a0e-113">Ayrıca `IKey`, bu anahtara bağlı bir [ıauthenticatedencryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği oluşturmak için kullanılabilecek bir `CreateEncryptor` yöntemi kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f6a0e-114">Ayrıca `IKey`, bu anahtara bağlı bir [ıauthenticatedencryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği oluşturmak için kullanılabilecek bir `CreateEncryptorInstance` yöntemi kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="f6a0e-115">Ham şifreleme malzemesini bir `IKey` örneğinden almak için API yok.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="f6a0e-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="f6a0e-116">IKeyManager</span></span>

<span data-ttu-id="f6a0e-117">`IKeyManager` arabirimi, genel anahtar depolama, alma ve düzenleme işleminden sorumlu bir nesneyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="f6a0e-118">Bu, üç üst düzey işlemini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="f6a0e-119">Yeni bir anahtar oluşturun ve depolama için kalıcı.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="f6a0e-120">Tüm anahtarları, depolama alanından alma.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-120">Get all keys from storage.</span></span>

* <span data-ttu-id="f6a0e-121">Bir veya daha fazla anahtarı iptal edin ve depolama için iptal bilgilerini kalıcı hale.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="f6a0e-122">`IKeyManager` yazmak çok gelişmiş bir görevdir ve geliştiricilerin çoğunluğu bunu denememelidir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="f6a0e-123">Bunun yerine, çoğu Geliştirici [Xmlkeymanager](#xmlkeymanager) sınıfı tarafından sunulan tesislerin avantajlarından yararlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="f6a0e-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="f6a0e-124">XmlKeyManager</span></span>

<span data-ttu-id="f6a0e-125">`XmlKeyManager` türü, `IKeyManager`'ın yerleşik somut uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="f6a0e-126">Bu anahtar emanet ve anahtarları, bekleyen veri şifrelemesi gibi birçok yararlı olanakları sağlar.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="f6a0e-127">Bu sistemdeki anahtarlar XML öğeleri (özellikle, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)) olarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="f6a0e-128">`XmlKeyManager` görevlerini yerine getirdikten sonra diğer birçok bileşene bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="f6a0e-129">Yeni anahtarlar tarafından kullanılan algoritmaları belirleyen `AlgorithmConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="f6a0e-130">`IXmlRepository`, anahtarların depolamada nerede kalıcı olduğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="f6a0e-131">bekleyen anahtarları şifrelemeye izin veren [optional] `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="f6a0e-132">anahtar emanet hizmetleri sağlayan `IKeyEscrowSink` [isteğe bağlı].</span><span class="sxs-lookup"><span data-stu-id="f6a0e-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="f6a0e-133">`IXmlRepository`, anahtarların depolamada nerede kalıcı olduğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="f6a0e-134">bekleyen anahtarları şifrelemeye izin veren [optional] `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="f6a0e-135">anahtar emanet hizmetleri sağlayan `IKeyEscrowSink` [isteğe bağlı].</span><span class="sxs-lookup"><span data-stu-id="f6a0e-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="f6a0e-136">Aşağıda, bu bileşenlerin `XmlKeyManager`içinde nasıl birbirine bağlanacağını gösteren üst düzey diyagramlar bulunur.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation2.png)

<span data-ttu-id="f6a0e-138">*Anahtar oluşturma/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="f6a0e-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="f6a0e-139">`CreateNewKey`uygulamasında `AlgorithmConfiguration` bileşeni, daha sonra XML olarak seri hale getirilen benzersiz bir `IAuthenticatedEncryptorDescriptor`oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="f6a0e-140">Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="f6a0e-141">Şifrelenmemiş XML, şifrelenmiş XML belgesi oluşturmak için bir `IXmlEncryptor` (gerekliyse) ile çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="f6a0e-142">Bu şifrelenmiş belge, `IXmlRepository`aracılığıyla uzun süreli depolamaya devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="f6a0e-143">(`IXmlEncryptor` yapılandırılmamışsa, şifrelenmemiş belge `IXmlRepository`kalıcı hale getirilir.)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Anahtar alma](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation1.png)

<span data-ttu-id="f6a0e-146">*Anahtar oluşturma/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="f6a0e-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="f6a0e-147">`CreateNewKey`uygulamasında `IAuthenticatedEncryptorConfiguration` bileşeni, daha sonra XML olarak seri hale getirilen benzersiz bir `IAuthenticatedEncryptorDescriptor`oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="f6a0e-148">Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="f6a0e-149">Şifrelenmemiş XML, şifrelenmiş XML belgesi oluşturmak için bir `IXmlEncryptor` (gerekliyse) ile çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="f6a0e-150">Bu şifrelenmiş belge, `IXmlRepository`aracılığıyla uzun süreli depolamaya devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="f6a0e-151">(`IXmlEncryptor` yapılandırılmamışsa, şifrelenmemiş belge `IXmlRepository`kalıcı hale getirilir.)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Anahtar alma](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="f6a0e-153">*Anahtar alımı/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="f6a0e-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="f6a0e-154">`GetAllKeys`uygulamasında, anahtarları temsil eden XML belgeleri ve geri alınamaz, temel `IXmlRepository`okundu.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="f6a0e-155">Bu belgeler şifrelenir, sistem otomatik olarak bunların şifresini çözer.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="f6a0e-156">`XmlKeyManager`, `IAuthenticatedEncryptorDescriptor` belgelerin serisini kaldırmak için uygun `IAuthenticatedEncryptorDescriptorDeserializer` örnekleri oluşturur, böylece tek tek `IKey` örneklerine sarmalanır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="f6a0e-157">Bu `IKey` örnekleri koleksiyonu çağırana döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="f6a0e-158">Belirli XML öğeleri hakkında daha fazla bilgi, [anahtar depolama biçimi belgesinde](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="f6a0e-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-159">IXmlRepository</span></span>

<span data-ttu-id="f6a0e-160">`IXmlRepository` arabirimi, bir yedekleme deposundan XML 'i kalıcı hale getirebilen ve XML alabileceği bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="f6a0e-161">Bu, iki API'lerini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-161">It exposes two APIs:</span></span>

* <span data-ttu-id="f6a0e-162">`GetAllElements`:`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="f6a0e-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="f6a0e-163">`IXmlRepository` uygulamalarının, bu nesnelerin üzerinden geçen XML 'yi ayrıştırması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="f6a0e-164">XML belgeleri donuk ele almanız ve belgelerini ayrıştırma ve oluşturma hakkında endişe daha yüksek katmanları olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="f6a0e-165">`IXmlRepository`uygulayan dört yerleşik somut tür vardır:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="f6a0e-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="f6a0e-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="f6a0e-168">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="f6a0e-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="f6a0e-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="f6a0e-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="f6a0e-172">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="f6a0e-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f6a0e-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="f6a0e-174">Daha fazla bilgi için bkz. [anahtar depolama sağlayıcıları belgesi](xref:security/data-protection/implementation/key-storage-providers) .</span><span class="sxs-lookup"><span data-stu-id="f6a0e-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="f6a0e-175">Özel bir `IXmlRepository` kaydetmek, farklı bir yedekleme deposu (örneğin, Azure Tablo Depolaması) kullanılırken uygundur.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="f6a0e-176">Varsayılan depo uygulama genelinde değiştirmek için özel bir `IXmlRepository` örneğini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="f6a0e-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f6a0e-177">IXmlEncryptor</span></span>

<span data-ttu-id="f6a0e-178">`IXmlEncryptor` arabirimi, düz metin XML öğesini şifreleyemeyen bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="f6a0e-179">Bu, tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-179">It exposes a single API:</span></span>

* <span data-ttu-id="f6a0e-180">(XElement plaintextElement) şifrelemek: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="f6a0e-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="f6a0e-181">Serileştirilmiş bir `IAuthenticatedEncryptorDescriptor` "şifreleme gerektirir" olarak işaretlenen herhangi bir öğe içeriyorsa, `XmlKeyManager` bu öğeleri yapılandırılmış `IXmlEncryptor``Encrypt` yöntemi aracılığıyla çalıştırır ve düz metin `IXmlRepository`öğesi yerine şifreli öğeyi kalıcı hale gelir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="f6a0e-182">`Encrypt` yönteminin çıktısı bir `EncryptedXmlInfo` nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="f6a0e-183">Bu nesne, hem sonuç veren `XElement` hem de karşılık gelen öğeyi şifre altına almak için kullanılabilecek bir `IXmlDecryptor` temsil eden tür içeren bir sarmalayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="f6a0e-184">`IXmlEncryptor`uygulayan dört yerleşik somut tür vardır:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="f6a0e-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f6a0e-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="f6a0e-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f6a0e-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="f6a0e-187">Dpapikselmlencryptor</span><span class="sxs-lookup"><span data-stu-id="f6a0e-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="f6a0e-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f6a0e-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="f6a0e-189">Daha fazla bilgi için [rest belgesinde anahtar şifrelemeyi](xref:security/data-protection/implementation/key-encryption-at-rest) inceleyin.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="f6a0e-190">Varsayılan anahtar şifreleme-Rest mekanizmasını uygulama genelinde değiştirmek için özel bir `IXmlEncryptor` örneğini kaydedin:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="f6a0e-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="f6a0e-191">IXmlDecryptor</span></span>

<span data-ttu-id="f6a0e-192">`IXmlDecryptor` arabirimi, bir `IXmlEncryptor`ile şifreleme olan bir `XElement` şifresini çözmeyi bilen bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="f6a0e-193">Bu, tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-193">It exposes a single API:</span></span>

* <span data-ttu-id="f6a0e-194">(XElement encryptedElement) şifresini: XElement</span><span class="sxs-lookup"><span data-stu-id="f6a0e-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="f6a0e-195">`Decrypt` yöntemi, `IXmlEncryptor.Encrypt`tarafından gerçekleştirilen şifrelemeyi geri alır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="f6a0e-196">Genellikle, her somut `IXmlEncryptor` uygulama karşılık gelen somut `IXmlDecryptor` uygulamasına sahip olur.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="f6a0e-197">`IXmlDecryptor` uygulayan türler aşağıdaki iki ortak oluşturucudan birine sahip olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="f6a0e-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="f6a0e-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="f6a0e-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="f6a0e-200">Oluşturucuya geçirilen `IServiceProvider` null olabilir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="f6a0e-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="f6a0e-201">IKeyEscrowSink</span></span>

<span data-ttu-id="f6a0e-202">`IKeyEscrowSink` arabirimi, hassas bilgileri Emanet gerçekleştirebilen bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="f6a0e-203">Seri hale getirilmiş tanımlayıcıların gizli bilgiler (örneğin, şifreleme malzemeleri) içerebileceğini ve bu, ilk yerde [ıxmlencryptor](#ixmlencryptor) türünün giriş ile ilgili olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="f6a0e-204">Ancak, Kazalar olabilir ve anahtar halkaları silinebilir veya bozulmuş.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="f6a0e-205">Emanet arabirimi, bir acil durum çıkış taraması sağlar ve bu, yapılandırılmış [ıxmlencryptor](#ixmlencryptor)tarafından dönüştürülmeden önce ham serileştirilmiş XML erişimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="f6a0e-206">Arabirim, tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="f6a0e-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="f6a0e-207">Store (GUID Keyıd, XElement öğe)</span><span class="sxs-lookup"><span data-stu-id="f6a0e-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="f6a0e-208">İş ilkesiyle güvenli bir şekilde tutarlı bir şekilde, sunulan öğeyi işlemek için `IKeyEscrowSink` uygulamasına kadar.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="f6a0e-209">Mümkün olan bir uygulama,, sertifikanın özel anahtarının esutde olduğu bilinen bir kurumsal X. 509.440 sertifikası kullanarak XML öğesini şifrelemek üzere, Emanet havuzu için olabilir. `CertificateXmlEncryptor` türü bu yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="f6a0e-210">`IKeyEscrowSink` uygulama, Ayrıca, belirtilen öğeyi uygun şekilde kalıcı hale getirmekten sorumludur.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="f6a0e-211">Varsayılan olarak, sunucu yöneticileri [bunu genel olarak yapılandırabilse](xref:security/data-protection/configuration/machine-wide-policy)de bir emanet mekanizması etkinleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="f6a0e-212">Ayrıca, aşağıdaki örnekte gösterildiği gibi `IDataProtectionBuilder.AddKeyEscrowSink` yöntemi aracılığıyla programlı bir şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="f6a0e-213">`AddKeyEscrowSink` yöntemi, `IKeyEscrowSink` örneklerinin tekton olmasını amaçlandığı için `IServiceCollection.AddSingleton` ve `IServiceCollection.AddInstance` aşırı yüklerini yansıtır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="f6a0e-214">Birden çok `IKeyEscrowSink` örneği kayıtlıysa, her biri anahtar oluşturma sırasında çağrılır, bu nedenle anahtarlar birden çok mekanizmayı eşzamanlı olarak gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="f6a0e-215">`IKeyEscrowSink` örneğinden malzeme okuma API 'SI yoktur.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="f6a0e-216">Emanet mekanizmasının tasarım kuramsal ile tutarlı budur: anahtar malzemesi güvenilir bir yetkili tarafından erişilebilir kılın yönelik ve uygulamanın kendisini güvenilir bir yetkili olmadığından, kendi escrowed malzeme erişimi olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="f6a0e-217">Aşağıdaki örnek kod, anahtarların yalnızca "CONTOSODomain Admins" üyelerinin kurtarabileceği şekilde bir `IKeyEscrowSink` oluşturulmasını ve kaydolduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a0e-218">Bu örneği çalıştırmak için bir etki alanına katılmış Windows 8'olmalıdır. / Windows Server 2012 makine ve etki alanı denetleyicisi Windows Server 2012 veya üzeri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f6a0e-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
