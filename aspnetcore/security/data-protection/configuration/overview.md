---
title: ASP.NET Core veri korumasını yapılandırma
author: rick-anderson
description: ASP.NET Core veri korumasını yapılandırma konusunda bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: ee43427fa1e82a365d49df50567b4ca7afb5a5d3
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902993"
---
# <a name="configure-aspnet-core-data-protection"></a>ASP.NET Core veri korumasını yapılandırma

Veri koruma sisteminde başlatıldığında, geçerli [varsayılan ayarları](xref:security/data-protection/configuration/default-settings) işletimsel ortamı hakkında temel. Bu ayarlar genellikle tek bir makinede çalışan uygulamalar için uygundur. Burada varsayılan ayarları değiştirmek için bir geliştirici isteyebilirsiniz durumlar vardır:

* Uygulama, birden fazla makine arasında yayılır.
* Uyumluluk nedenleriyle.

Bu senaryolar için veri koruma sisteminde zengin yapılandırma API sunar.

> [!WARNING]
> Benzer şekilde yapılandırma dosyalarını, veri koruma anahtarı halka uygun izinleri kullanarak korunmalıdır. Bekleyen anahtarlarını şifrelemek seçebilirsiniz, ancak bu saldırganlar yeni anahtarları oluşturmanızı engellemez. Sonuç olarak, uygulamanızın güvenlik etkilenir. Veri koruma ile yapılandırılmış depolama konumu uygulamanın kendi, benzer şekilde, yapılandırma dosyalarını korur sınırlı erişimini olması gerekir. Örneğin, diskte, anahtar halkası depolamayı seçerseniz, dosya sistemi izinlerini kullanın. Yalnızca kimliği altında emin olun, web uygulamanızın çalıştırdığı okuma, yazma ve bu dizine erişim oluşturun. Yalnızca web uygulaması, Azure tablo depolama hizmetini kullanıyorsanız, okuma, yazma veya yeni girişler, tablo depolama, vb. oluşturma olanağı sahip olmalıdır.
>
> Genişletme yöntemi [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) döndürür bir [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` genişletme yöntemleri birlikte veri korumayı yapılandırmak için seçenekleri bağlayabilirsiniz(ekleyebilirsiniz) olduğunu gösterir.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

İçindeki anahtarları depolamak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), sistemle yapılandırma [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) içinde `Startup` sınıfı:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Anahtar halkası depolama konumunu ayarlayın (örneğin, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). Konum, arama için ayarlamanız gerekir `ProtectKeysWithAzureKeyVault` uygulayan bir [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) , devre dışı bırakır anahtar halkası depolama konumu dahil olmak üzere otomatik veri koruma ayarları. Yukarıdaki örnekte, anahtar halkası kalıcı hale getirmek için Azure Blob Depolama kullanır. Daha fazla bilgi için [anahtar depolama sağlayıcıları: Azure ve Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). Anahtar halkası ile yerel olarak da devam edebilir [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

`keyIdentifier` Anahtar şifreleme için kullanılan anahtar kasası anahtar tanımlayıcısı. Örneğin, adlı anahtar Kasası'nda oluşturulmuş bir anahtar `dataprotection` içinde `contosokeyvault` anahtar tanımlayıcısı `https://contosokeyvault.vault.azure.net/keys/dataprotection/`. Uygulamayla sağlamak **Unwrap Key** ve **Wrap Key** anahtar kasasındaki izinleri.

`ProtectKeysWithAzureKeyVault` aşırı yüklemeler:

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) kullanımına izin veren bir [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) key vault'u kullanma veri koruma sisteminde etkinleştirmek için.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) kullanımına izin veren bir `ClientId` ve [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) veri koruma sisteminde key vault'u kullanma olanağı.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) kullanımına izin veren bir `ClientId` ve `ClientSecret` veri koruma sisteminde key vault'u kullanma olanağı.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Bir UNC paylaşımında anahtarları depolamak için *% LOCALAPPDATA %* varsayılan konumu, sistemle yapılandırma [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Anahtar kalıcılığı yerini değiştirirseniz, DPAPI uygun şifreleme mekanizması olup olmadığını bilmez olduğundan sistem anahtarları bekleyen artık otomatik olarak şifreler.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Sistem herhangi birini çağırarak bekleyen anahtarlarınızı korumak için yapılandırabileceğiniz [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) yapılandırma API'leri. Bir UNC paylaşımında anahtarlarını depolar ve belirli bir X.509 sertifikasıyla bekleyen Bu anahtarları şifreler aşağıdaki örnekte, göz önünde bulundurun:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 veya daha sonra sağlayabilir bir [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) için [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)gibi bir dosyadan bir sertifika yüklenir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

Bkz: [, anahtar şifreleme Rest](xref:security/data-protection/implementation/key-encryption-at-rest) örnekler ve tartışma için yerleşik anahtar şifreleme mekanizmaları hakkında daha fazla.

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

ASP.NET Core 2.1 veya daha sonra sertifika döndürmeyi ve bir dizi kullanılarak, bekleme sırasında anahtarlarının şifresini [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) ile sertifikaları [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Bir anahtar yaşam süresi 14 gün yerine varsayılan 90 gün kullanılacak sistemini yapılandırmak için kullanın [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Aynı fiziksel anahtar deposu paylaşıyorsanız bile varsayılan olarak, başka bir içerik kök yollarına bağlı uygulamalardan veri koruma sisteminde yalıtır. Bu uygulamalar, diğer kişilerin korumalı yüklerin anlamadan engeller.

Korumalı yüklerini uygulamalar arasında paylaşmak için:

* Yapılandırma <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> her uygulamada aynı değere sahip.
* Uygulamalar arasında veri koruma API'si yığını aynı sürümünü kullanın. Gerçekleştirmek **ya da** uygulamaların proje dosyalarında biri:
  * Aynı paylaşılan çerçeve sürümü aracılığıyla başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).
  * Aynı başvuru [veri koruma paket](xref:security/data-protection/introduction#package-layout) sürümü.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Burada, sona erme yaklaştıkça (yeni anahtar oluştur) anahtarları otomatik olarak geri almak için bir uygulama istemediğiniz bir senaryo olabilir. Bunun bir örneği burada yalnızca birincil uygulama ile ilgili anahtar yönetimi konuları sorumludur ve ikincil uygulamaları yalnızca bir salt okunur anahtar halkası görüntüleyebilmek bir birincil/ikincil ilişkide, ayarlanan uygulamalar olabilir. İkincil uygulamaları sistemiyle yapılandırarak anahtar halkası salt okunur işlemek için yapılandırılabilir <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Uygulama başına yalıtımı

Veri koruma sisteminde bir ASP.NET Core ana bilgisayar tarafından sağlandığında, bu uygulamaları aynı çalışan işlem hesabı altında çalıştığından ve aynı ana anahtar malzemesini kullanıyorsanız bile otomatik olarak birbirinden, uygulamaları yalıtır. Bu System.Web 's IsolateApps değiştiricisi için biraz benzer `<machineKey>` öğesi.

Yalıtım mekanizması çalıştığı yerel makinede bulunan her bir uygulama benzersiz bir kiracı, bu nedenle dikkate alarak <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> belirli bir uygulamanın uygulama Kimliğini bir ayrıştırıcı olarak otomatik olarak içerir. kök erişim izni verilmemiş. Uygulamanın benzersiz Kimliğini uygulamanın fiziksel yoludur:

* Barındırılan uygulamalar için [IIS](xref:fundamentals/servers/index#iis-http-server), uygulamayı IIS fiziksel yolunu benzersiz kimliğidir. Bir uygulama bir web çiftliği ortamında dağıttıysanız, IIS ortamları benzer şekilde web grubundaki tüm makinelerdeki yapılandırıldığını varsayarak, bu değer kararlı olur.
* Şirket içinde barındırılan uygulamaları üzerinde çalışan [Kestrel sunucu](xref:fundamentals/servers/index#kestrel), diskteki uygulamasının fiziksel yolu benzersiz kimliğidir.

Benzersiz tanımlayıcı sıfırlar varlığını sürdürmesi için tasarlanmış&mdash;hem tek tek uygulama ve makine.

Uygulamaları kötü amaçlı olmayan bu yalıtım mekanizması varsayar. Kötü amaçlı bir uygulama her zaman aynı çalışan işlem hesabı altında çalışan diğer herhangi bir uygulamayı etkiler. Uygulamaları birbirini güvenilmeyen olduğu bir paylaşılan barındırma ortamında, barındırma sağlayıcısı, anahtar deposu uygulamaların temel ayırarak dahil olmak üzere, uygulamalar arasında işletim sistemi düzeyinde yalıtımı sağlamak üzere önlem almanız gerekir.

Veri koruma sisteminde bir ASP.NET Core ana bilgisayar tarafından sağlanan değildir (örneğin, aracılığıyla örneği oluşturursanız `DataProtectionProvider` somut tür) uygulama yalıtımı, varsayılan olarak devre dışıdır. Uygulama yalıtımı devre dışı bırakıldığında, uygun sağladıkları sürece tüm uygulamalar aynı anahtar malzemesini tarafından desteklenen yükleri paylaşabilirsiniz [amacıyla](xref:security/data-protection/consumer-apis/purpose-strings). Bu ortamda uygulama yalıtımı sağlamak için çağrı [SetApplicationName](#setapplicationname) yöntemi yapılandırma nesnesi ve her uygulama için benzersiz bir ad belirtin.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>UseCryptographicAlgorithms algoritmalarıyla değiştirme

Veri koruma yığın, yeni oluşturulan anahtarlar tarafından kullanılan varsayılan algoritma değiştirmenizi sağlar. Bunu yapmanın en kolay yolu çağırmaktır [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) yapılandırmasını geri çağırma gelen:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

' % S'varsayılan EncryptionAlgorithm AES 256 CBC ve ValidationAlgorithm HMACSHA256 varsayılandır. Varsayılan ilke bir sistem yöneticisi tarafından ayarlanabilir bir [makineye ilke](xref:security/data-protection/configuration/machine-wide-policy), ancak açık çağrı `UseCryptographicAlgorithms` varsayılan ilkesini geçersiz kılar.

Çağırma `UseCryptographicAlgorithms` önceden tanımlanmış bir yerleşik listeden istenilen algoritma belirtmenizi sağlar. Algoritmanın uygulamasının hakkında endişelenmeniz gerekmez. Yukarıdaki senaryoda, veri koruma sisteminde Windows üzerinde çalışıyorsa AES CNG uygulamasını kullanmayı dener. Aksi takdirde, bu geri yönetilen döner [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) sınıfı.

Bir uygulama için bir çağrı aracılığıyla el ile belirtebilirsiniz [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Algoritmaların değiştirilmesi, mevcut anahtar halkası anahtarlarında etkilemez. Yalnızca yeni oluşturulan anahtarları etkiler.

### <a name="specifying-custom-managed-algorithms"></a>Özel bir yönetilen algoritmaları belirtme

::: moniker range=">= aspnetcore-2.0"

Özel bir yönetilen algoritmaları belirtmek için oluşturun bir [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) uygulama türlerine işaret örneği:

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Özel bir yönetilen algoritmaları belirtmek için oluşturun bir [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) uygulama türlerine işaret örneği:

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

::: moniker-end

Genellikle \*türü özellikleri somut için işaret etmelidir (aracılığıyla, Ortak parametresiz bir ctor) instantiable uygulamaları [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) ve [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ancak Sistem özel-çalışmaları gibi bazı değerler `typeof(Aes)` kolaylık sağlamak için.

> [!NOTE]
> SymmetricAlgorithm ≥ 128 bit anahtar uzunluğuna ve ≥ 64 bit bir blok boyutu olmalıdır ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir. Bir Özet boyutunu KeyedHashAlgorithm olmalıdır > = 128 bit ve karma algoritmasının ait Özet uzunluğa eşit uzunlukta anahtarları desteklemesi gerekir. KeyedHashAlgorithm HMAC olarak kesinlikle gerekli değildir.

### <a name="specifying-custom-windows-cng-algorithms"></a>Özel Windows CNG algoritmaları belirtme

::: moniker range=">= aspnetcore-2.0"

İle HMAC doğrulama CBC modunda Şifrelemesi'ni kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) algoritmik bilgileri içeren örneği:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

İle HMAC doğrulama CBC modunda Şifrelemesi'ni kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) algoritmik bilgileri içeren örneği:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

::: moniker-end

> [!NOTE]
> Anahtar uzunluğu simetrik blok şifreleme algoritması olmalıdır > = 128 bit, bir blok boyutu > = 64 bit ve PKCS #7 doldurma CBC modunda şifrelemeyle desteklemesi gerekir. Karma algoritması bir Özet boyutu olmalıdır > = 128 bit ve BCRYPT ile açılmasını desteklemelidir\_Algoritma\_İŞLEMEK\_HMAC\_BAYRAĞI bayrağı. \*Sağlayıcısı özellikleri ayarlanabilir belirtilen algoritma için varsayılan sağlayıcıyı kullanmak için null. Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için belgelere bakın.

::: moniker range=">= aspnetcore-2.0"

İle doğrulama Galois/sayacı şifrelemesi kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) algoritmik bilgileri içeren örneği:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

İle doğrulama Galois/sayacı şifrelemesi kullanarak özel bir Windows CNG algoritması belirtmek için oluşturun bir [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) algoritmik bilgileri içeren örneği:

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

::: moniker-end

> [!NOTE]
> Anahtar uzunluğu simetrik blok şifreleme algoritması olmalıdır > = 128 bit, 128 bit tam olarak bir blok boyutu ve GCM şifreleme desteklemesi gerekir. Ayarlayabileceğiniz [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) özelliğini belirtilen algoritma için varsayılan sağlayıcıyı kullanmak için null. Bkz: [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) daha fazla bilgi için belgelere bakın.

### <a name="specifying-other-custom-algorithms"></a>Özel diğer algoritmalar belirtme

Birinci sınıf bir API olarak açığa değil de, veri koruma sisteminde algoritması neredeyse her türlü belirtilmesine izin verecek şekilde genişletilebilir. Örneğin, bir donanım güvenlik modülü (HSM) içinde bulunan tüm anahtarlar tutmak ve çekirdeğin özel bir uygulama, şifreleme ve şifre çözme rutinlerini mümkündür. Bkz: [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) içinde [çekirdek şifreleme genişletilebilirliği](xref:security/data-protection/extensibility/core-crypto) daha fazla bilgi için.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Bir Docker kapsayıcısında barındırırken kalıcı anahtarları

İçinde barındırırken bir [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kapsayıcı, anahtarlar saklanabilir ya da:

* Bir paylaşılan veya bir konak monte biriminde gibi kapsayıcının yaşam ötesine devam eden bir Docker birim bir klasör.
* Bir dış sağlayıcı gibi [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) veya [Redis](https://redis.io/).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
