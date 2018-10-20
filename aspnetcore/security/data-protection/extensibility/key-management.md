---
title: ASP.NET Core anahtar yönetimi genişletilebilirliği
author: rick-anderson
description: ASP.NET Core veri koruma anahtar yönetimi genişletilebilirliği hakkında bilgi edinin.
ms.author: riande
ms.date: 11/22/2017
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: b52212ff3462748a5c64f21e1b7854673e5fcadc
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477468"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>ASP.NET Core anahtar yönetimi genişletilebilirliği

> [!TIP]
> Okuma [anahtar yönetimi](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) bölümden önce bu bölümü okunurken gibi bazı bu API'leri temel kavramları açıklar.

> [!WARNING]
> Aşağıdaki arabirimlerinden birini uygulayan türler, iş parçacığı açısından güvenli olmalıdır birden çok arayanlar için.

## <a name="key"></a>Anahtar

`IKey` Cryptosystem anahtarında temel gösterimini arabirimidir. Terim anahtarı burada "şifreleme anahtar malzemesi" değişmez değer duygusu değil, soyut anlamında kullanılmıştır. Bir anahtarı aşağıdaki özelliklere sahiptir:

* Etkinleştirme, oluşturma ve sona erme tarihleri

* İptal durumu

* Anahtar tanımlayıcısını (GUID)

::: moniker range=">= aspnetcore-2.0"

Ayrıca, `IKey` sunan bir `CreateEncryptor` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ayrıca, `IKey` sunan bir `CreateEncryptorInstance` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.

::: moniker-end

> [!NOTE]
> Ham şifreleme malzemesini almak için hiçbir API yoktur bir `IKey` örneği.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Arabirimi genel anahtar depolama alanı, alma ve düzenleme için sorumlu bir nesneyi temsil eder. Bu, üç üst düzey işlemini kullanıma sunar:

* Yeni bir anahtar oluşturun ve depolama için kalıcı.

* Tüm anahtarları, depolama alanından alma.

* Bir veya daha fazla anahtarı iptal edin ve depolama için iptal bilgilerini kalıcı hale.

>[!WARNING]
> Yazma bir `IKeyManager` çok gelişmiş bir görevdir ve geliştiricilerin çoğu, çalışmayın. Bunun yerine, çoğu geliştirici tarafından sunulan özellikleri yararlanmak [XmlKeyManager](#xmlkeymanager) sınıfı.

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` Türüdür yerleşik somut uygulamasını `IKeyManager`. Bu anahtar emanet ve anahtarları, bekleyen veri şifrelemesi gibi birçok yararlı olanakları sağlar. Bu sistemde anahtarları XML öğeleri olarak temsil edilir (özellikle [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` görevleri yerine getirerek sırasında birkaç diğer bileşenlere bağlıdır:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, yeni anahtarlar tarafından kullanılan algoritmalar belirler.

* `IXmlRepository`, burada anahtarları kalıcı depolama alanında hangi denetimleri.

* `IXmlEncryptor` [isteğe bağlı], anahtarları, bekleyen veri şifreleme sağlar.

* `IKeyEscrowSink` [isteğe bağlı], anahtar emanet hizmetleri sağlar.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, burada anahtarları kalıcı depolama alanında hangi denetimleri.

* `IXmlEncryptor` [isteğe bağlı], anahtarları, bekleyen veri şifreleme sağlar.

* `IKeyEscrowSink` [isteğe bağlı], anahtar emanet hizmetleri sağlar.

::: moniker-end

Nasıl bu bileşenler birlikte içinde kablolu arabirimlerdir gösteren yüksek düzey şemalarını aşağıda verilmiştir `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation2.png)

*Anahtar oluşturma / CreateNewKey*

Uygulamasında `CreateNewKey`, `AlgorithmConfiguration` benzersiz bir oluşturmak için kullanılan bileşen `IAuthenticatedEncryptorDescriptor`, hangi sonra serileştirilmiş XML olarak. Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır. Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş bir XML belgesi oluşturmak için. Şifrelenmiş bu belgenin uzun vadeli depolama için kalıcı `IXmlRepository`. (Hiçbir `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge içinde kalıcı `IXmlRepository`.)

![Anahtar alma](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Anahtar oluşturma](key-management/_static/keycreation1.png)

*Anahtar oluşturma / CreateNewKey*

Uygulamasında `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` benzersiz bir oluşturmak için kullanılan bileşen `IAuthenticatedEncryptorDescriptor`, hangi sonra serileştirilmiş XML olarak. Ham (şifrelenmemiş) XML anahtar emanet havuz varsa, havuz için uzun vadeli depolama için sağlanır. Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş bir XML belgesi oluşturmak için. Şifrelenmiş bu belgenin uzun vadeli depolama için kalıcı `IXmlRepository`. (Hiçbir `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge içinde kalıcı `IXmlRepository`.)

![Anahtar alma](key-management/_static/keyretrieval1.png)

::: moniker-end

*Anahtar alma / GetAllKeys*

Uygulamasında `GetAllKeys`, XML belgeleri temsil eden tuşları ve geri alma işlemleri temel okunur `IXmlRepository`. Bu belgeler şifrelenir, sistem otomatik olarak bunların şifresini çözer. `XmlKeyManager` uygun oluşturur `IAuthenticatedEncryptorDescriptorDeserializer` belgeleri seri durumdan çıkarılacak örnekleri yeniden `IAuthenticatedEncryptorDescriptor` örnekleri, sonra ayrı ayrı sarmalanır `IKey` örnekleri. Bu koleksiyonu `IKey` örnekleri, çağırana döndürülür.

Belirli bir XML öğeleri hakkında daha fazla bilgi bulunabilir [anahtar depolama biçimi belge](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Arabirimi XML kalıcı hale getirmek ve yedekleme deposundan XML almak bir türü temsil eder. Bu, iki API'lerini kullanıma sunar:

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Uygulamaları `IXmlRepository` aracılığıyla geçirme XML'i ayrıştırmakta gerekmez. XML belgeleri donuk ele almanız ve belgelerini ayrıştırma ve oluşturma hakkında endişe daha yüksek katmanları olanak tanır.

Uygulayan dört yerleşik somut tür `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Bkz: [anahtar depolama sağlayıcıları belge](xref:security/data-protection/implementation/key-storage-providers) daha fazla bilgi için.

Özel bir kayıt `IXmlRepository` farklı yedekleme deposu (örneğin, Azure tablo depolama) kullanılırken uygundur.

Varsayılan depo birçok farklı uygulama değiştirmek için özel bir kayıt `IXmlRepository` örneği:

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

`IXmlEncryptor` Arabirimi düz metin XML öğesi şifreleyebilirsiniz bir türü temsil eder. Bu, tek bir API sunar:

* (XElement plaintextElement) şifrelemek: EncryptedXmlInfo

Seri hale getirilmiş bir, `IAuthenticatedEncryptorDescriptor` işaretlenmiş ardından "şifreleme gerektiren" tüm öğeleri içerir `XmlKeyManager` bu öğeleri yapılandırılan çalışacak `IXmlEncryptor`'s `Encrypt` yöntemi ve kalıcı hale gelir enciphered öğe yerine düz metin öğesine `IXmlRepository`. Çıkışı `Encrypt` yöntemi bir `EncryptedXmlInfo` nesne. Bu nesne enciphered hem sonuç içeren bir sarmalayıcı olan `XElement` ve türünü temsil eden bir `IXmlDecryptor` karşılık gelen öğe çözmek için kullanılabilir.

Uygulayan dört yerleşik somut tür `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Bkz: [rest belge, anahtar şifreleme](xref:security/data-protection/implementation/key-encryption-at-rest) daha fazla bilgi için.

Varsayılan bekleyen anahtar şifreleme mekanizması birçok farklı uygulama değiştirmek için özel bir kayıt `IXmlEncryptor` örneği:

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

`IXmlDecryptor` Arabirimi şifresini çözmek bildiği bir türü temsil eder bir `XElement` , enciphered aracılığıyla bir `IXmlEncryptor`. Bu, tek bir API sunar:

* (XElement encryptedElement) şifresini: XElement

`Decrypt` Yöntemi tarafından gerçekleştirilen şifreleme alır `IXmlEncryptor.Encrypt`. Genellikle, her somut `IXmlEncryptor` uygulamasına karşılık gelen bir somut sahipseniz `IXmlDecryptor` uygulaması.

Türleri uygulayan `IXmlDecryptor` aşağıdaki iki genel oluşturucular biri olmalıdır:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` Geçirilen oluşturucuya null olabilir.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Arabirimi hassas bilgilerin emanet gerçekleştirebileceği bir türü temsil eder. Seri hale getirilmiş tanımlayıcıları (şifreleme malzemelerini gibi) hassas bilgiler içerebilir ve ne için giriş neden budur geri çağırma [IXmlEncryptor](#ixmlencryptor) ilk başta yazın. Ancak, Kazalar olabilir ve anahtar halkaları silinebilir veya bozulmuş.

Yapılandırılmış tüm tarafından dönüştürülür önce ham serileştirilmiş XML erişimine bir Acil Durum kaçış noktası, emanet arabirimidir [IXmlEncryptor](#ixmlencryptor). Arabirim, tek bir API sunar:

* Store (GUID Keyıd, XElement öğe)

En fazla olan `IKeyEscrowSink` belirtilen öğe işletme ilkesiyle uyumlu güvenli bir şekilde işlemek üzere uygulama. Sertifikanın özel anahtarı bir yere kalacakları XML öğesi bilinen bir kurumsal X.509 sertifikası kullanarak şifrelemek emanet havuz için bir olası uygulama olabilir; `CertificateXmlEncryptor` türü bu konuda yardımcı olabilir. `IKeyEscrowSink` Uygulamasıdır ayrıca belirtilen öğe uygun şekilde kalıcı hale getirmekten sorumludur.

Sunucu yöneticileri olabilir ancak varsayılan olarak emanet bir mekanizma, etkin [bu genel yapılandırma](xref:security/data-protection/configuration/machine-wide-policy). Ayrıca programlı olarak aracılığıyla yapılandırılabilir `IDataProtectionBuilder.AddKeyEscrowSink` aşağıdaki örnekte gösterildiği gibi yöntemi. `AddKeyEscrowSink` Yöntemi aşırı yüklemeleri yansıtma `IServiceCollection.AddSingleton` ve `IServiceCollection.AddInstance` aşırı olarak `IKeyEscrowSink` örnekleri teklileri olmasını yöneliktir. Birden çok `IKeyEscrowSink` örnekleri kayıtlı, her biri anahtar oluşturma sırasında çağrılacak şekilde anahtarları için birden fazla mekanizmayı aynı anda kalacakları.

Malzemesini okumak için hiçbir API yoktur bir `IKeyEscrowSink` örneği. Emanet mekanizmasının tasarım kuramsal ile tutarlı budur: anahtar malzemesi güvenilir bir yetkili tarafından erişilebilir kılın yönelik ve uygulamanın kendisini güvenilir bir yetkili olmadığından, kendi escrowed malzeme erişimi olmamalıdır.

Aşağıdaki örnek kod oluşturma ve kaydetme işlemlerini gösterir bir `IKeyEscrowSink` burada anahtarları kalacakları sağlayacak şekilde yalnızca "CONTOSODomain Admins" üyeleri bunları kurtarabilirsiniz.

> [!NOTE]
> Bu örneği çalıştırmak için bir etki alanına katılmış Windows 8'olmalıdır. / Windows Server 2012 makine ve etki alanı denetleyicisi Windows Server 2012 veya üzeri olması gerekir.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
