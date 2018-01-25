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
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a>Anahtar Yönetimi genişletilebilirliği

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Okuma [anahtar yönetimi](../implementation/key-management.md#data-protection-implementation-key-management) bazı bu API'leri temel kavramları açıklar gibi bu bölümü okumadan önce bölümü.

>[!WARNING]
> Aşağıdaki arabirimleri hiçbirini uygulama türleri iş parçacığı açısından güvenli olması için birden çok arayanlar.

## <a name="key"></a>Anahtar

`IKey` Cryptosystem anahtarında temel gösterimini bir arabirimdir. Terim anahtar burada soyut anlamda, değil "şifreleme anahtar malzemesi" değişmez değer duygusu kullanılır. Bir anahtarı aşağıdaki özelliklere sahiptir:

* Etkinleştirme, oluşturma ve sona erme tarihleri

* İptal durumu

* Anahtar tanımlayıcı (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ayrıca, `IKey` kullanıma sunan bir `CreateEncryptor` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ayrıca, `IKey` kullanıma sunan bir `CreateEncryptorInstance` oluşturmak için kullanılan yöntemi bir [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) örneği bu anahtara bağlı.

---

> [!NOTE]
> Ham şifreleme malzemesini almak için hiçbir API yoktur bir `IKey` örneği.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Arabirimi genel anahtarı depolama, alma ve düzenleme sorumlu bir nesneyi temsil eder. Üç üst düzey işlemi sunar:

* Yeni bir anahtar oluşturun ve depolama birimine kalır.

* Tüm anahtarları depolama biriminden alın.

* Bir veya daha fazla anahtarı iptal edin ve depolama için iptal bilgilerini kalır.

>[!WARNING]
> Yazma bir `IKeyManager` çok gelişmiş bir görevdir ve geliştiricilerin çoğunluğu onu çalışmayın. Bunun yerine, çoğu geliştirici tarafından sunulan olanaklarının yararlanmalıdır [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) sınıfı.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` Türü uygulamasıdır yerleşik somut `IKeyManager`. Anahtar emanet ve şifreleme anahtarlarının REST dahil olmak üzere çeşitli yararlı olanakları sağlar. Bu sistemdeki anahtarları XML öğeleri temsil (özellikle [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager`görevleri yerine getirerek esnasında çeşitli bileşenleri bağlıdır:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, yeni anahtarlar tarafından kullanılan algoritmalar belirler.

* `IXmlRepository`, burada anahtarları kalıcı depolama alanına hangi denetimlerin.

* `IXmlEncryptor`[isteğe bağlı], şifreleme anahtarları REST izin verir.

* `IKeyEscrowSink`[isteğe bağlı], anahtar emanet hizmetleri sağlar.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, burada anahtarları kalıcı depolama alanına hangi denetimlerin.

* `IXmlEncryptor`[isteğe bağlı], şifreleme anahtarları REST izin verir.

* `IKeyEscrowSink`[isteğe bağlı], anahtar emanet hizmetleri sağlar.

---

Aşağıda nasıl bu bileşenlerin birlikte içinde kablolu arabirimlerdir gösteren yüksek düzey şemalarını olan `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Anahtar oluşturma](key-management/_static/keycreation2.png)

   *Anahtar oluşturma / CreateNewKey*

Uygulamasında `CreateNewKey`, `AlgorithmConfiguration` bileşen benzersiz bir oluşturmak için kullanılan `IAuthenticatedEncryptorDescriptor`, sonra serileştirildiği XML olarak. Bir anahtar emanet havuzu varsa, ham (şifrelenmemiş) XML havuz için uzun vadeli depolama için sağlanır. Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş XML belgesi oluşturulamadı. Bu şifrelenmiş belge uzun vadeli depolama için kalıcı `IXmlRepository`. (Yoksa `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge kalıcı hale getirilir `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Anahtar oluşturma](key-management/_static/keycreation1.png)

   *Anahtar oluşturma / CreateNewKey*

Uygulamasında `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` bileşen benzersiz bir oluşturmak için kullanılan `IAuthenticatedEncryptorDescriptor`, sonra serileştirildiği XML olarak. Bir anahtar emanet havuzu varsa, ham (şifrelenmemiş) XML havuz için uzun vadeli depolama için sağlanır. Şifrelenmemiş XML ardından çalıştırın bir `IXmlEncryptor` (gerekliyse) şifrelenmiş XML belgesi oluşturulamadı. Bu şifrelenmiş belge uzun vadeli depolama için kalıcı `IXmlRepository`. (Yoksa `IXmlEncryptor` olan yapılandırılmış, şifrelenmemiş belge kalıcı hale getirilir `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Anahtar alma](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Anahtar alma](key-management/_static/keyretrieval1.png)

---

   *Key Retrieval / GetAllKeys*

Uygulamasında `GetAllKeys`temsil eden anahtarları XML belgeleri ve iptalleri arka plandaki gelen okunur `IXmlRepository`. Bu belgeler şifrelenir, sistem otomatik olarak onları şifresini. `XmlKeyManager`uygun oluşturur `IAuthenticatedEncryptorDescriptorDeserializer` belgeleri seri durumdan çıkarılacak örnekleri yeniden `IAuthenticatedEncryptorDescriptor` sonra tek tek Sarmalanan örnekleri `IKey` örnekleri. Bu koleksiyonu `IKey` örnekleri çağırana döndürülür.

Özel XML öğeleri hakkında daha fazla bilgi bulunabilir [anahtar depolama biçimi belge](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Arabirimi XML kalıcı hale getirmek ve yedekleme depolama alanından XML almak bir türü temsil eder. İki API'lerini gösterir:

* GetAllElements() : IReadOnlyCollection<XElement>

* StoreElement (XElement öğesi, dize friendlyName)

Uygulamaları `IXmlRepository` bunlardan geçen XML Ayrıştırma gerek yoktur. XML belgeleri opak kabul ve daha yüksek katmanları oluşturmak ve belgeleri ayrıştırma hakkında endişelenmeniz olanak tanır.

Arabirimini uygulayan iki yerleşik somut türü `IXmlRepository`: `FileSystemXmlRepository` ve `RegistryXmlRepository`. Bkz: [anahtar depolama sağlayıcıları belge](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) daha fazla bilgi için. Özel bir kaydetme `IXmlRepository` örn, bir başka yedekleme deposu kullanmak için uygun şekilde Azure Blob Storage olacaktır.

Varsayılan depo uygulama genelinde değiştirmek için özel bir kayıt `IXmlRepository` örneği:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor` Arabirimi bir düz metin XML öğesi şifreleyebilirsiniz bir türü temsil eder. Tek bir API sunar:

* (XElement plaintextElement) şifrelemek: EncryptedXmlInfo

Seri hale getirilmiş bir varsa `IAuthenticatedEncryptorDescriptor` "şifreleme gerektiren", ardından işaretlenen herhangi bir öğe içeren `XmlKeyManager` bu öğeleri yapılandırılmış çalışacak `IXmlEncryptor`'s `Encrypt` yöntemi enciphered öğesi kalıcı yerine düz metin öğesine `IXmlRepository`. Çıktısını `Encrypt` yöntemi bir `EncryptedXmlInfo` nesnesi. Bu nesne enciphered her iki sonuç içeren bir sarmalayıcı olan `XElement` ve temsil eden tür bir `IXmlDecryptor` karşılık gelen öğe çözmek için kullanılabilir.

Arabirimini uygulayan dört yerleşik somut türleri vardır `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

Bkz: [rest belge anahtar şifreleme](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) daha fazla bilgi için.

Varsayılan rest adresindeki anahtar şifreleme mekanizmasını uygulama genelinde değiştirmek için özel bir kayıt `IXmlEncryptor` örneği:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor` Arabirimi şifresini çözmek bildiği bir türü temsil eden bir `XElement` , enciphered aracılığıyla bir `IXmlEncryptor`. Tek bir API sunar:

* (XElement encryptedElement) şifresini: XElement

`Decrypt` Yöntemi tarafından gerçekleştirilen şifreleme alır `IXmlEncryptor.Encrypt`. Genellikle, her somut `IXmlEncryptor` uygulaması ilgili somut sahip `IXmlDecryptor` uygulaması.

Türleri uygulayan `IXmlDecryptor` aşağıdaki iki ortak oluşturuculardan birine sahip olmalıdır:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` Geçirilen Oluşturucusu null olabilir.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Arabirimi hassas bilgilerin emanet gerçekleştirebileceğiniz bir türü temsil eder. Serileştirilmiş tanımlayıcıları (örneğin, şifreleme malzemesi) hassas bilgiler içerebilir ve bu ne ssql neden olan geri çağırma [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) ilk başta yazın. Ancak, KAZALARDAN dolayı RID'ler gerçekleşir ve anahtar çalma silinebilir veya bozulmuş.

Emanet arabirimi herhangi tarafından yapılandırılan dönüştürüldükten önce ham serileştirilmiş XML izin veren bir Acil Durum kaçış tarama sağlar [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). Arabirim tek bir API sunar:

* Store(Guid keyId, XElement element)

En fazla olan `IKeyEscrowSink` sağlanan öğe iş ilkesiyle tutarlı güvenli şekilde işlemek üzere uygulama. Sertifikanın özel anahtarı bir yere kalacakları bilinen bir kurumsal X.509 sertifikası kullanarak XML öğesi şifrelemek emanet havuz için bir olası uygulama olabilir; `CertificateXmlEncryptor` türü bu yardımcı olabilir. `IKeyEscrowSink` Uygulamasıdır ayrıca sağlanan öğe uygun şekilde sürdürmek için sorumlu.

Sunucu yöneticileri olabilir ancak varsayılan olarak hiçbir emanet mekanizması, etkin [bu genel yapılandırma](xref:security/data-protection/configuration/machine-wide-policy). Ayrıca program aracılığıyla aracılığıyla yapılandırılabilir `IDataProtectionBuilder.AddKeyEscrowSink` aşağıdaki örnekte gösterildiği gibi yöntemi. `AddKeyEscrowSink` Yöntemi aşırı yansıtma `IServiceCollection.AddSingleton` ve `IServiceCollection.AddInstance` aşırı yüklemelerinden olarak `IKeyEscrowSink` örnekleri teklileri olmasını yöneliktir. Birden çok `IKeyEscrowSink` örnekleri kayıtlı, anahtarları için birden fazla mekanizmayı aynı anda kalacakları şekilde her biri anahtar oluşturma sırasında çağrılır.

Malzemesini okumak için hiçbir API yoktur bir `IKeyEscrowSink` örneği. Bu emanet mekanizması tasarım teorik ile tutarlıdır: anahtar malzemesi güvenilir bir yetkili tarafından erişilebilir hedeflenen ve uygulama kendisini güvenilir bir yetkili olmadığından, kendi escrowed malzemeler için erişim olmamalıdır.

Aşağıdaki örnek kod oluşturma ve kaydetme gösteren bir `IKeyEscrowSink` burada anahtarları kalacakları sağlayacak şekilde yalnızca "CONTOSODomain yöneticileri" üyeleri kurtarma yapabilirsiniz.

> [!NOTE]
> Bu örneği çalıştırmak için bir etki alanına katılmış Windows 8'de olmalıdır / Windows Server 2012 makine ve etki alanı denetleyicisinin Windows Server 2012 veya üzeri olması gerekir.

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
