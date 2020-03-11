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
# <a name="key-management-extensibility-in-aspnet-core"></a>ASP.NET Core anahtar yönetimi genişletilebilirliği

> [!TIP]
> Bu API 'nin arkasındaki temel kavramların bazılarını açıklandığı için, bu bölümü okumadan önce [anahtar yönetimi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) bölümünü okuyun.

> [!WARNING]
> Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.

## <a name="key"></a>Anahtar

`IKey` arabirimi, Cryptosystem içindeki bir anahtarın temel gösterimidir. Terim anahtarı burada "şifreleme anahtar malzemesi" değişmez değer duygusu değil, soyut anlamında kullanılmıştır. Bir anahtarı aşağıdaki özelliklere sahiptir:

* Etkinleştirme, oluşturma ve sona erme tarihleri

* İptal durumu

* Anahtar tanımlayıcısını (GUID)

::: moniker range=">= aspnetcore-2.0"

Ayrıca `IKey`, bu anahtara bağlı bir [ıauthenticatedencryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği oluşturmak için kullanılabilecek bir `CreateEncryptor` yöntemi kullanıma sunar.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ayrıca `IKey`, bu anahtara bağlı bir [ıauthenticatedencryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği oluşturmak için kullanılabilecek bir `CreateEncryptorInstance` yöntemi kullanıma sunar.

::: moniker-end

> [!NOTE]
> Ham şifreleme malzemesini bir `IKey` örneğinden almak için API yok.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` arabirimi, genel anahtar depolama, alma ve düzenleme işleminden sorumlu bir nesneyi temsil eder. Bu, üç üst düzey işlemini kullanıma sunar:

* Yeni bir anahtar oluşturun ve depolama için kalıcı.

* Tüm anahtarları, depolama alanından alma.

* Bir veya daha fazla anahtarı iptal edin ve depolama için iptal bilgilerini kalıcı hale.

>[!WARNING]
> `IKeyManager` yazmak çok gelişmiş bir görevdir ve geliştiricilerin çoğunluğu bunu denememelidir. Bunun yerine, çoğu Geliştirici [Xmlkeymanager](#xmlkeymanager) sınıfı tarafından sunulan tesislerin avantajlarından yararlanmalıdır.

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` türü, `IKeyManager`'ın yerleşik somut uygulamasıdır. Bu anahtar emanet ve anahtarları, bekleyen veri şifrelemesi gibi birçok yararlı olanakları sağlar. Bu sistemdeki anahtarlar XML öğeleri (özellikle, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)) olarak temsil edilir.

`XmlKeyManager` görevlerini yerine getirdikten sonra diğer birçok bileşene bağlıdır:

::: moniker range=">= aspnetcore-2.0"

* Yeni anahtarlar tarafından kullanılan algoritmaları belirleyen `AlgorithmConfiguration`.

* `IXmlRepository`, anahtarların depolamada nerede kalıcı olduğunu denetler.

* bekleyen anahtarları şifrelemeye izin veren [optional] `IXmlEncryptor`.

* anahtar emanet hizmetleri sağlayan `IKeyEscrowSink` [isteğe bağlı].

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, anahtarların depolamada nerede kalıcı olduğunu denetler.

* bekleyen anahtarları şifrelemeye izin veren [optional] `IXmlEncryptor`.

* anahtar emanet hizmetleri sağlayan `IKeyEscrowSink` [isteğe bağlı].

::: moniker-end

Aşağıda, bu bileşenlerin `XmlKeyManager`içinde nasıl birbirine bağlanacağını gösteren üst düzey diyagramlar bulunur.

::: moniker range=">= aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation2.png)

*Anahtar oluşturma/CreateNewKey*

`CreateNewKey`uygulamasında `AlgorithmConfiguration` bileşeni, daha sonra XML olarak seri hale getirilen benzersiz bir `IAuthenticatedEncryptorDescriptor`oluşturmak için kullanılır. Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır. Şifrelenmemiş XML, şifrelenmiş XML belgesi oluşturmak için bir `IXmlEncryptor` (gerekliyse) ile çalıştırılır. Bu şifrelenmiş belge, `IXmlRepository`aracılığıyla uzun süreli depolamaya devam ediyor. (`IXmlEncryptor` yapılandırılmamışsa, şifrelenmemiş belge `IXmlRepository`kalıcı hale getirilir.)

![Anahtar alma](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation1.png)

*Anahtar oluşturma/CreateNewKey*

`CreateNewKey`uygulamasında `IAuthenticatedEncryptorConfiguration` bileşeni, daha sonra XML olarak seri hale getirilen benzersiz bir `IAuthenticatedEncryptorDescriptor`oluşturmak için kullanılır. Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır. Şifrelenmemiş XML, şifrelenmiş XML belgesi oluşturmak için bir `IXmlEncryptor` (gerekliyse) ile çalıştırılır. Bu şifrelenmiş belge, `IXmlRepository`aracılığıyla uzun süreli depolamaya devam ediyor. (`IXmlEncryptor` yapılandırılmamışsa, şifrelenmemiş belge `IXmlRepository`kalıcı hale getirilir.)

![Anahtar alma](key-management/_static/keyretrieval1.png)

::: moniker-end

*Anahtar alımı/GetAllKeys*

`GetAllKeys`uygulamasında, anahtarları temsil eden XML belgeleri ve geri alınamaz, temel `IXmlRepository`okundu. Bu belgeler şifrelenir, sistem otomatik olarak bunların şifresini çözer. `XmlKeyManager`, `IAuthenticatedEncryptorDescriptor` belgelerin serisini kaldırmak için uygun `IAuthenticatedEncryptorDescriptorDeserializer` örnekleri oluşturur, böylece tek tek `IKey` örneklerine sarmalanır. Bu `IKey` örnekleri koleksiyonu çağırana döndürülür.

Belirli XML öğeleri hakkında daha fazla bilgi, [anahtar depolama biçimi belgesinde](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format)bulunabilir.

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` arabirimi, bir yedekleme deposundan XML 'i kalıcı hale getirebilen ve XML alabileceği bir türü temsil eder. Bu, iki API'lerini kullanıma sunar:

* `GetAllElements`:`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

`IXmlRepository` uygulamalarının, bu nesnelerin üzerinden geçen XML 'yi ayrıştırması gerekmez. XML belgeleri donuk ele almanız ve belgelerini ayrıştırma ve oluşturma hakkında endişe daha yüksek katmanları olanak tanır.

`IXmlRepository`uygulayan dört yerleşik somut tür vardır:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Daha fazla bilgi için bkz. [anahtar depolama sağlayıcıları belgesi](xref:security/data-protection/implementation/key-storage-providers) .

Özel bir `IXmlRepository` kaydetmek, farklı bir yedekleme deposu (örneğin, Azure Tablo Depolaması) kullanılırken uygundur.

Varsayılan depo uygulama genelinde değiştirmek için özel bir `IXmlRepository` örneğini kaydedin:

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

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor` arabirimi, düz metin XML öğesini şifreleyemeyen bir türü temsil eder. Bu, tek bir API sunar:

* (XElement plaintextElement) şifrelemek: EncryptedXmlInfo

Serileştirilmiş bir `IAuthenticatedEncryptorDescriptor` "şifreleme gerektirir" olarak işaretlenen herhangi bir öğe içeriyorsa, `XmlKeyManager` bu öğeleri yapılandırılmış `IXmlEncryptor``Encrypt` yöntemi aracılığıyla çalıştırır ve düz metin `IXmlRepository`öğesi yerine şifreli öğeyi kalıcı hale gelir. `Encrypt` yönteminin çıktısı bir `EncryptedXmlInfo` nesnesidir. Bu nesne, hem sonuç veren `XElement` hem de karşılık gelen öğeyi şifre altına almak için kullanılabilecek bir `IXmlDecryptor` temsil eden tür içeren bir sarmalayıcıdır.

`IXmlEncryptor`uygulayan dört yerleşik somut tür vardır:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [Dpapikselmlencryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Daha fazla bilgi için [rest belgesinde anahtar şifrelemeyi](xref:security/data-protection/implementation/key-encryption-at-rest) inceleyin.

Varsayılan anahtar şifreleme-Rest mekanizmasını uygulama genelinde değiştirmek için özel bir `IXmlEncryptor` örneğini kaydedin:

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

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor` arabirimi, bir `IXmlEncryptor`ile şifreleme olan bir `XElement` şifresini çözmeyi bilen bir türü temsil eder. Bu, tek bir API sunar:

* (XElement encryptedElement) şifresini: XElement

`Decrypt` yöntemi, `IXmlEncryptor.Encrypt`tarafından gerçekleştirilen şifrelemeyi geri alır. Genellikle, her somut `IXmlEncryptor` uygulama karşılık gelen somut `IXmlDecryptor` uygulamasına sahip olur.

`IXmlDecryptor` uygulayan türler aşağıdaki iki ortak oluşturucudan birine sahip olmalıdır:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> Oluşturucuya geçirilen `IServiceProvider` null olabilir.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` arabirimi, hassas bilgileri Emanet gerçekleştirebilen bir türü temsil eder. Seri hale getirilmiş tanımlayıcıların gizli bilgiler (örneğin, şifreleme malzemeleri) içerebileceğini ve bu, ilk yerde [ıxmlencryptor](#ixmlencryptor) türünün giriş ile ilgili olduğunu unutmayın. Ancak, Kazalar olabilir ve anahtar halkaları silinebilir veya bozulmuş.

Emanet arabirimi, bir acil durum çıkış taraması sağlar ve bu, yapılandırılmış [ıxmlencryptor](#ixmlencryptor)tarafından dönüştürülmeden önce ham serileştirilmiş XML erişimine izin verir. Arabirim, tek bir API sunar:

* Store (GUID Keyıd, XElement öğe)

İş ilkesiyle güvenli bir şekilde tutarlı bir şekilde, sunulan öğeyi işlemek için `IKeyEscrowSink` uygulamasına kadar. Mümkün olan bir uygulama,, sertifikanın özel anahtarının esutde olduğu bilinen bir kurumsal X. 509.440 sertifikası kullanarak XML öğesini şifrelemek üzere, Emanet havuzu için olabilir. `CertificateXmlEncryptor` türü bu yardımcı olabilir. `IKeyEscrowSink` uygulama, Ayrıca, belirtilen öğeyi uygun şekilde kalıcı hale getirmekten sorumludur.

Varsayılan olarak, sunucu yöneticileri [bunu genel olarak yapılandırabilse](xref:security/data-protection/configuration/machine-wide-policy)de bir emanet mekanizması etkinleştirilmez. Ayrıca, aşağıdaki örnekte gösterildiği gibi `IDataProtectionBuilder.AddKeyEscrowSink` yöntemi aracılığıyla programlı bir şekilde yapılandırılabilir. `AddKeyEscrowSink` yöntemi, `IKeyEscrowSink` örneklerinin tekton olmasını amaçlandığı için `IServiceCollection.AddSingleton` ve `IServiceCollection.AddInstance` aşırı yüklerini yansıtır. Birden çok `IKeyEscrowSink` örneği kayıtlıysa, her biri anahtar oluşturma sırasında çağrılır, bu nedenle anahtarlar birden çok mekanizmayı eşzamanlı olarak gerçekleştirebilir.

`IKeyEscrowSink` örneğinden malzeme okuma API 'SI yoktur. Emanet mekanizmasının tasarım kuramsal ile tutarlı budur: anahtar malzemesi güvenilir bir yetkili tarafından erişilebilir kılın yönelik ve uygulamanın kendisini güvenilir bir yetkili olmadığından, kendi escrowed malzeme erişimi olmamalıdır.

Aşağıdaki örnek kod, anahtarların yalnızca "CONTOSODomain Admins" üyelerinin kurtarabileceği şekilde bir `IKeyEscrowSink` oluşturulmasını ve kaydolduğunu gösterir.

> [!NOTE]
> Bu örneği çalıştırmak için bir etki alanına katılmış Windows 8'olmalıdır. / Windows Server 2012 makine ve etki alanı denetleyicisi Windows Server 2012 veya üzeri olması gerekir.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
