---
title: "Anahtar Yönetimi genişletilebilirliği"
author: rick-anderson
description: "Bu belge, ASP.NET Core veri koruma anahtar yönetimi genişletilebilirliği özetlenmektedir."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 2bd0f945306a7acd6252c2a00aafb648cf722616
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="key-management-extensibility"></a><span data-ttu-id="252b7-103">Anahtar Yönetimi genişletilebilirliği</span><span class="sxs-lookup"><span data-stu-id="252b7-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="252b7-104">Okuma [anahtar yönetimi](../implementation/key-management.md#data-protection-implementation-key-management) bazı bu API'leri temel kavramları açıklar gibi bu bölümü okumadan önce bölümü.</span><span class="sxs-lookup"><span data-stu-id="252b7-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="252b7-105">Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.</span><span class="sxs-lookup"><span data-stu-id="252b7-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="252b7-106">Anahtar</span><span class="sxs-lookup"><span data-stu-id="252b7-106">Key</span></span>

<span data-ttu-id="252b7-107">`IKey` Cryptosystem anahtarında temel gösterimini bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="252b7-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="252b7-108">Terim anahtar burada soyut anlamda, değil "şifreleme anahtar malzemesi" değişmez değer duygusu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="252b7-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="252b7-109">Bir anahtarı aşağıdaki özelliklere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="252b7-109">A key has the following properties:</span></span>

* <span data-ttu-id="252b7-110">Etkinleştirme, oluşturma ve sona erme tarihleri</span><span class="sxs-lookup"><span data-stu-id="252b7-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="252b7-111">İptal durumu</span><span class="sxs-lookup"><span data-stu-id="252b7-111">Revocation status</span></span>

* <span data-ttu-id="252b7-112">Anahtar tanımlayıcı (GUID)</span><span class="sxs-lookup"><span data-stu-id="252b7-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="252b7-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="252b7-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="252b7-114">Ayrıca, `IKey` kullanıma sunan bir `CreateEncryptor` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.</span><span class="sxs-lookup"><span data-stu-id="252b7-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="252b7-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="252b7-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="252b7-116">Ayrıca, `IKey` kullanıma sunan bir `CreateEncryptorInstance` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.</span><span class="sxs-lookup"><span data-stu-id="252b7-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="252b7-117">Ham şifreleme malzemesini almak için hiçbir API yoktur bir `IKey` örneği.</span><span class="sxs-lookup"><span data-stu-id="252b7-117">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="252b7-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="252b7-118">IKeyManager</span></span>

<span data-ttu-id="252b7-119">`IKeyManager` Arabirimi genel anahtarı depolama, alma ve düzenleme sorumlu bir nesneyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="252b7-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="252b7-120">Üç üst düzey işlemi sunar:</span><span class="sxs-lookup"><span data-stu-id="252b7-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="252b7-121">Yeni bir anahtar oluşturun ve depolama birimine kalır.</span><span class="sxs-lookup"><span data-stu-id="252b7-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="252b7-122">Tüm anahtarları depolama biriminden alın.</span><span class="sxs-lookup"><span data-stu-id="252b7-122">Get all keys from storage.</span></span>

* <span data-ttu-id="252b7-123">Bir veya daha fazla anahtarı iptal edin ve depolama için iptal bilgilerini kalır.</span><span class="sxs-lookup"><span data-stu-id="252b7-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="252b7-124">Yazma bir `IKeyManager` çok gelişmiş bir görevdir ve geliştiricilerin çoğunluğu çalışmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="252b7-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="252b7-125">Bunun yerine, çoğu geliştirici tarafından sunulan olanaklarının yararlanmalıdır [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="252b7-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="252b7-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="252b7-126">XmlKeyManager</span></span>

<span data-ttu-id="252b7-127">`XmlKeyManager` Türü uygulamasıdır yerleşik somut `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="252b7-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="252b7-128">Anahtar emanet ve şifreleme anahtarlarının REST dahil olmak üzere çeşitli yararlı olanakları sağlar.</span><span class="sxs-lookup"><span data-stu-id="252b7-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="252b7-129">Bu sistemdeki anahtarları XML öğeleri temsil (özellikle [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="252b7-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="252b7-130">`XmlKeyManager`görevleri yerine getirerek esnasında çeşitli bileşenleri bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="252b7-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="252b7-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="252b7-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="252b7-132">`AlgorithmConfiguration`, yeni anahtarlar tarafından kullanılan algoritmalar belirler.</span><span class="sxs-lookup"><span data-stu-id="252b7-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="252b7-133">`IXmlRepository`, burada anahtarları kalıcı depolama alanına hangi denetimlerin.</span><span class="sxs-lookup"><span data-stu-id="252b7-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="252b7-134">`IXmlEncryptor`[isteğe bağlı], şifreleme anahtarları REST izin verir.</span><span class="sxs-lookup"><span data-stu-id="252b7-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="252b7-135">`IKeyEscrowSink`[isteğe bağlı], anahtar emanet hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="252b7-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="252b7-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="252b7-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="252b7-137">`IXmlRepository`, burada anahtarları kalıcı depolama alanına hangi denetimlerin.</span><span class="sxs-lookup"><span data-stu-id="252b7-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="252b7-138">`IXmlEncryptor`[isteğe bağlı], şifreleme anahtarları REST izin verir.</span><span class="sxs-lookup"><span data-stu-id="252b7-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="252b7-139">`IKeyEscrowSink`[isteğe bağlı], anahtar emanet hizmetleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="252b7-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="252b7-140">Aşağıda nasıl bu bileşenlerin birlikte içinde kablolu arabirimlerdir gösteren yüksek düzey şemalarını olan `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="252b7-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="252b7-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="252b7-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Anahtar oluşturma](key-management/_static/keycreation2.png)

   <span data-ttu-id="252b7-143">*Anahtar oluşturma / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="252b7-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="252b7-144">Uygulamasında `CreateNewKey`, `AlgorithmConfiguration` bileşen benzersiz bir oluşturmak için kullanılan `IAuthenticatedEncryptorDescriptor`, sonra serileştirildiği XML olarak.</span><span class="sxs-lookup"><span data-stu-id="252b7-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="252b7-145">Bir anahtar emanet havuzu varsa, ham (şifrelenmemiş) XML havuz için uzun vadeli depolama için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="252b7-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="252b7-146">Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş XML belgesi oluşturulamadı.</span><span class="sxs-lookup"><span data-stu-id="252b7-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="252b7-147">Bu şifrelenmiş belge uzun vadeli depolama için kalıcı `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="252b7-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="252b7-148">(Yoksa `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge kalıcı hale getirilir `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="252b7-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="252b7-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="252b7-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Anahtar oluşturma](key-management/_static/keycreation1.png)

   <span data-ttu-id="252b7-151">*Anahtar oluşturma / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="252b7-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="252b7-152">Uygulamasında `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` bileşen benzersiz bir oluşturmak için kullanılan `IAuthenticatedEncryptorDescriptor`, sonra serileştirildiği XML olarak.</span><span class="sxs-lookup"><span data-stu-id="252b7-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="252b7-153">Bir anahtar emanet havuzu varsa, ham (şifrelenmemiş) XML havuz için uzun vadeli depolama için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="252b7-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="252b7-154">Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş XML belgesi oluşturulamadı.</span><span class="sxs-lookup"><span data-stu-id="252b7-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="252b7-155">Bu şifrelenmiş belge uzun vadeli depolama için kalıcı `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="252b7-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="252b7-156">(Yoksa `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge kalıcı hale getirilir `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="252b7-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="252b7-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="252b7-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Anahtar alma](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="252b7-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="252b7-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Anahtar alma](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="252b7-161">*Key Retrieval / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="252b7-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="252b7-162">Uygulamasında `GetAllKeys`temsil eden anahtarları XML belgeleri ve iptalleri arka plandaki gelen okunur `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="252b7-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="252b7-163">Bu belgeler şifrelenir, sistem otomatik olarak onları şifresini.</span><span class="sxs-lookup"><span data-stu-id="252b7-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="252b7-164">`XmlKeyManager`uygun oluşturur `IAuthenticatedEncryptorDescriptorDeserializer` belgeleri seri durumdan çıkarılacak örnekleri yeniden `IAuthenticatedEncryptorDescriptor` sonra tek tek Sarmalanan örnekleri `IKey` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="252b7-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="252b7-165">Bu koleksiyonu `IKey` örnekleri çağırana döndürülür.</span><span class="sxs-lookup"><span data-stu-id="252b7-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="252b7-166">Özel XML öğeleri hakkında daha fazla bilgi bulunabilir [anahtar depolama biçimi belge](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="252b7-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="252b7-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="252b7-167">IXmlRepository</span></span>

<span data-ttu-id="252b7-168">`IXmlRepository` Arabirimi XML kalıcı hale getirmek ve yedekleme depolama alanından XML almak bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="252b7-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="252b7-169">İki API'lerini gösterir:</span><span class="sxs-lookup"><span data-stu-id="252b7-169">It exposes two APIs:</span></span>

* <span data-ttu-id="252b7-170">GetAllElements() : IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="252b7-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="252b7-171">StoreElement (XElement öğesi, dize friendlyName)</span><span class="sxs-lookup"><span data-stu-id="252b7-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="252b7-172">Uygulamaları `IXmlRepository` bunlardan geçen XML Ayrıştırma gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="252b7-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="252b7-173">XML belgeleri opak kabul ve daha yüksek katmanları oluşturmak ve belgeleri ayrıştırma hakkında endişelenmeniz olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="252b7-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="252b7-174">Arabirimini uygulayan iki yerleşik somut türü `IXmlRepository`: `FileSystemXmlRepository` ve `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="252b7-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="252b7-175">Bkz: [anahtar depolama sağlayıcıları belge](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="252b7-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="252b7-176">Özel bir kaydetme `IXmlRepository` örn, bir başka yedekleme deposu kullanmak için uygun şekilde Azure Blob Storage olacaktır.</span><span class="sxs-lookup"><span data-stu-id="252b7-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="252b7-177">Varsayılan depo uygulama genelinde değiştirmek için özel bir kayıt `IXmlRepository` örneği:</span><span class="sxs-lookup"><span data-stu-id="252b7-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="252b7-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="252b7-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="252b7-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="252b7-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="252b7-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="252b7-180">IXmlEncryptor</span></span>

<span data-ttu-id="252b7-181">`IXmlEncryptor` Arabirimi bir düz metin XML öğesi şifreleyebilirsiniz bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="252b7-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="252b7-182">Tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="252b7-182">It exposes a single API:</span></span>

* <span data-ttu-id="252b7-183">(XElement plaintextElement) şifrelemek: EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="252b7-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="252b7-184">Seri hale getirilmiş bir varsa `IAuthenticatedEncryptorDescriptor` "şifreleme gerektiren", ardından işaretlenen herhangi bir öğe içeren `XmlKeyManager` bu öğeleri yapılandırılmış çalışacak `IXmlEncryptor`'s `Encrypt` yöntemi enciphered öğesi kalıcı yerine düz metin öğesine `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="252b7-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="252b7-185">Çıktısını `Encrypt` yöntemi bir `EncryptedXmlInfo` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="252b7-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="252b7-186">Bu nesne enciphered her iki sonuç içeren bir sarmalayıcı olan `XElement` ve temsil eden tür bir `IXmlDecryptor` karşılık gelen öğe çözmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="252b7-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="252b7-187">Arabirimini uygulayan dört yerleşik somut türleri vardır `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="252b7-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="252b7-188">Bkz: [rest belge anahtar şifreleme](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="252b7-188">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="252b7-189">Varsayılan rest adresindeki anahtar şifreleme mekanizmasını uygulama genelinde değiştirmek için özel bir kayıt `IXmlEncryptor` örneği:</span><span class="sxs-lookup"><span data-stu-id="252b7-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="252b7-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="252b7-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="252b7-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="252b7-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="252b7-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="252b7-192">IXmlDecryptor</span></span>

<span data-ttu-id="252b7-193">`IXmlDecryptor` Arabirimi şifresini çözmek bildiği bir türü temsil eden bir `XElement` , enciphered aracılığıyla bir `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="252b7-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="252b7-194">Tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="252b7-194">It exposes a single API:</span></span>

* <span data-ttu-id="252b7-195">(XElement encryptedElement) şifresini: XElement</span><span class="sxs-lookup"><span data-stu-id="252b7-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="252b7-196">`Decrypt` Yöntemi tarafından gerçekleştirilen şifreleme alır `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="252b7-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="252b7-197">Genellikle, her somut `IXmlEncryptor` uygulaması ilgili somut sahip `IXmlDecryptor` uygulaması.</span><span class="sxs-lookup"><span data-stu-id="252b7-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="252b7-198">Türleri uygulayan `IXmlDecryptor` aşağıdaki iki ortak oluşturuculardan birine sahip olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="252b7-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="252b7-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="252b7-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="252b7-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="252b7-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="252b7-201">`IServiceProvider` Geçirilen Oluşturucusu null olabilir.</span><span class="sxs-lookup"><span data-stu-id="252b7-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="252b7-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="252b7-202">IKeyEscrowSink</span></span>

<span data-ttu-id="252b7-203">`IKeyEscrowSink` Arabirimi hassas bilgilerin emanet gerçekleştirebileceğiniz bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="252b7-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="252b7-204">Serileştirilmiş tanımlayıcıları (örneğin, şifreleme malzemesi) hassas bilgiler içerebilir ve bu ne ssql neden olan geri çağırma [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) ilk başta yazın.</span><span class="sxs-lookup"><span data-stu-id="252b7-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="252b7-205">Ancak, KAZALARDAN dolayı RID'ler gerçekleşir ve anahtar çalma silinebilir veya bozulmuş.</span><span class="sxs-lookup"><span data-stu-id="252b7-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="252b7-206">Emanet arabirimi herhangi tarafından yapılandırılan dönüştürüldükten önce ham serileştirilmiş XML izin veren bir Acil Durum kaçış tarama sağlar [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="252b7-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="252b7-207">Arabirim tek bir API sunar:</span><span class="sxs-lookup"><span data-stu-id="252b7-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="252b7-208">Store(Guid keyId, XElement element)</span><span class="sxs-lookup"><span data-stu-id="252b7-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="252b7-209">En fazla olan `IKeyEscrowSink` sağlanan öğe iş ilkesiyle tutarlı güvenli şekilde işlemek üzere uygulama.</span><span class="sxs-lookup"><span data-stu-id="252b7-209">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="252b7-210">Sertifikanın özel anahtarı bir yere kalacakları bilinen bir kurumsal X.509 sertifikası kullanarak XML öğesi şifrelemek emanet havuz için bir olası uygulama olabilir; `CertificateXmlEncryptor` türü bu yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="252b7-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="252b7-211">`IKeyEscrowSink` Uygulamasıdır ayrıca sağlanan öğe uygun şekilde sürdürmek için sorumlu.</span><span class="sxs-lookup"><span data-stu-id="252b7-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="252b7-212">Sunucu yöneticileri olabilir ancak varsayılan olarak hiçbir emanet mekanizması, etkin [bu genel yapılandırma](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="252b7-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="252b7-213">Ayrıca program aracılığıyla aracılığıyla yapılandırılabilir `IDataProtectionBuilder.AddKeyEscrowSink` aşağıdaki örnekte gösterildiği gibi yöntemi.</span><span class="sxs-lookup"><span data-stu-id="252b7-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="252b7-214">`AddKeyEscrowSink` Yöntemi aşırı yansıtma `IServiceCollection.AddSingleton` ve `IServiceCollection.AddInstance` aşırı yüklemelerinden olarak `IKeyEscrowSink` örnekleri teklileri olmasını yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="252b7-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="252b7-215">Birden çok `IKeyEscrowSink` örnekleri kayıtlı, anahtarları için birden fazla mekanizmayı aynı anda kalacakları şekilde her biri anahtar oluşturma sırasında çağrılır.</span><span class="sxs-lookup"><span data-stu-id="252b7-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="252b7-216">Malzemesini okumak için hiçbir API yoktur bir `IKeyEscrowSink` örneği.</span><span class="sxs-lookup"><span data-stu-id="252b7-216">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="252b7-217">Bu emanet mekanizması tasarım teorik ile tutarlıdır: anahtar malzemesi güvenilir bir yetkili tarafından erişilebilir hedeflenen ve uygulama kendisini güvenilir bir yetkili olmadığından, kendi escrowed malzemeler için erişim olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="252b7-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="252b7-218">Aşağıdaki örnek kod oluşturma ve kaydetme gösteren bir `IKeyEscrowSink` burada anahtarları kalacakları sağlayacak şekilde yalnızca "CONTOSODomain yöneticileri" üyeleri kurtarma yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="252b7-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="252b7-219">Bu örneği çalıştırmak için bir etki alanına katılmış Windows 8'de olmalıdır / Windows Server 2012 makine ve etki alanı denetleyicisinin Windows Server 2012 veya üzeri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="252b7-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
